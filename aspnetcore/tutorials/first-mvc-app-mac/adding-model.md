---
title: "Přidat model do aplikace ASP.NET MVC jádra"
author: rick-anderson
description: "Přidáte model do jednoduchou aplikaci ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.devlang: csharp
ms.prod: .net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: bf4d5d289266b585cbdfbb70c7482620fd4ced54
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="313e7-103">Klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat** > **nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="313e7-103">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="313e7-104">V **nový soubor** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="313e7-104">In the **New File** dialog:</span></span>

  * <span data-ttu-id="313e7-105">Vyberte **Obecné** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="313e7-105">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="313e7-106">Vyberte **prázdné třídy** v problémové center.</span><span class="sxs-lookup"><span data-stu-id="313e7-106">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="313e7-107">Název třídy **film** a vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="313e7-107">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="313e7-108">Přidejte následující vlastnosti pro `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="313e7-108">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="313e7-109">`ID` Pole je vyžadováno pro databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="313e7-109">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="313e7-110">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="313e7-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="313e7-111">Nyní máte **M**odelu ve vaší **M**VC aplikace.</span><span class="sxs-lookup"><span data-stu-id="313e7-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="313e7-112">Příprava projektu pro generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="313e7-112">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="313e7-113">Klikněte pravým tlačítkem na soubor projektu a pak vyberte **nástroje > Upravit soubor**.</span><span class="sxs-lookup"><span data-stu-id="313e7-113">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![zobrazení výše krok](adding-model/_static/1.png)

- <span data-ttu-id="313e7-115">Přidejte následující zvýrazněnou balíčků NuGet *MvcMovie.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="313e7-115">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="313e7-116">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="313e7-116">Save the file.</span></span>

- <span data-ttu-id="313e7-117">Vytvoření *Models/MvcMovieContext.cs* souboru a přidejte následující `MvcMovieContext` třídy:[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="313e7-117">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="313e7-118">Otevřete *Startup.cs* souboru a přidejte dva direktiv Using:[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="313e7-118">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="313e7-119">Přidat kontext databáze, který má *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="313e7-119">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="313e7-120">Tato hodnota informuje Entity Framework, které třídy modelu jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="313e7-120">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="313e7-121">Definujete jeden *sady entit* film objektů, které bude možné vyjádřit v databázi jako film tabulku.</span><span class="sxs-lookup"><span data-stu-id="313e7-121">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="313e7-122">Sestavte projekt a ověřte, zda že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="313e7-122">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="313e7-123">Vygenerované uživatelské rozhraní MovieController</span><span class="sxs-lookup"><span data-stu-id="313e7-123">Scaffold the MovieController</span></span>

<span data-ttu-id="313e7-124">Otevřete okno terminálu ve složce projektu a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="313e7-124">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="313e7-125">Pokud dojde k chybě `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span><span class="sxs-lookup"><span data-stu-id="313e7-125">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="313e7-126">Pokud jste v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="313e7-126">You are in the project directory.</span></span> <span data-ttu-id="313e7-127">Má adresáři projektu *Program.cs*, *Startup.cs* a *.csproj* soubory.</span><span class="sxs-lookup"><span data-stu-id="313e7-127">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="313e7-128">Je vaše dotnet verze 1.1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="313e7-128">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="313e7-129">Spustit `dotnet` získat verzi.</span><span class="sxs-lookup"><span data-stu-id="313e7-129">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="313e7-130">Jste přidali `<DotNetCliToolReference>` elementu, který chcete [MvcMovie.csproj soubor](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="313e7-130">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="313e7-131">Modul generování uživatelského rozhraní vytvoří následující:</span><span class="sxs-lookup"><span data-stu-id="313e7-131">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="313e7-132">Řadič filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="313e7-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="313e7-133">Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="313e7-133">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="313e7-134">Automatické vytváření [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*.</span><span class="sxs-lookup"><span data-stu-id="313e7-134">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="313e7-135">Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.</span><span class="sxs-lookup"><span data-stu-id="313e7-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="313e7-136">Přidání souborů do sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="313e7-136">Add the files to Visual Studio</span></span>

* <span data-ttu-id="313e7-137">Přidat *MovieController.cs* souboru projektu sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="313e7-137">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="313e7-138">Klikněte pravým tlačítkem na *řadiče* složky a vyberte **Přidat > Přidat soubory**.</span><span class="sxs-lookup"><span data-stu-id="313e7-138">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="313e7-139">Vyberte *MovieController.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="313e7-139">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="313e7-140">Přidat *filmy* složek a zobrazení:</span><span class="sxs-lookup"><span data-stu-id="313e7-140">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="313e7-141">Klikněte pravým tlačítkem na *zobrazení* složky a vyberte **Přidat > Přidat existující složku**.</span><span class="sxs-lookup"><span data-stu-id="313e7-141">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="313e7-142">Přejděte na *zobrazení* složky, vyberte *Views\Movies*a potom vyberte **otevřete**.</span><span class="sxs-lookup"><span data-stu-id="313e7-142">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="313e7-143">V **vyberte soubory, které chcete přidat z filmy** vyberte **zahrnují všechny**a potom **OK**.</span><span class="sxs-lookup"><span data-stu-id="313e7-143">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="313e7-144">Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data.</span><span class="sxs-lookup"><span data-stu-id="313e7-144">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="313e7-145">V dalším kurzu jsme budete pracovat s databází.</span><span class="sxs-lookup"><span data-stu-id="313e7-145">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="313e7-146">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="313e7-146">Additional resources</span></span>

* [<span data-ttu-id="313e7-147">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="313e7-147">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="313e7-148">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="313e7-148">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="313e7-149">[Předchozí přidávání zobrazení](adding-view.md)
[další práci s SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="313e7-149">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
