---
title: "Přidání modelu do aplikace ASP.NET MVC jádra."
author: rick-anderson
description: "Přidáte model do jednoduchou aplikaci ASP.NET Core."
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, webové rozhraní API, REST, Mac, Linux, HTTP, služby, služba HTTP, VS Code"
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="663c2-104">Přidání třídy k *modely* složku s názvem *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="663c2-104">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="663c2-105">Přidejte následující kód, který *Models/Movie.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="663c2-105">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="663c2-106">`ID` Pole je vyžadováno pro databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="663c2-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="663c2-107">Vytvořit aplikaci pro ověření nemáte žádné chyby a nakonec jste přidali **M**odelu k vaší **M**VC aplikace.</span><span class="sxs-lookup"><span data-stu-id="663c2-107">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="663c2-108">Příprava projektu pro generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="663c2-108">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="663c2-109">Přidejte následující zvýrazněnou balíčků NuGet *MvcMovie.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="663c2-109">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="663c2-110">Uložte tento soubor a vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="663c2-110">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="663c2-111">Vytvoření *Models/MvcMovieContext.cs* souboru a přidejte následující `MvcMovieContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="663c2-111">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="663c2-112">Otevřete *Startup.cs* souboru a přidejte dva direktiv Using:</span><span class="sxs-lookup"><span data-stu-id="663c2-112">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="663c2-113">Přidat kontext databáze, který má *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="663c2-113">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="663c2-114">Tato hodnota informuje Entity Framework, které třídy modelu jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="663c2-114">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="663c2-115">Definujete jeden *sady entit* film objektů, které bude možné vyjádřit v databázi jako film tabulku.</span><span class="sxs-lookup"><span data-stu-id="663c2-115">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="663c2-116">Sestavte projekt a ověřte, zda že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="663c2-116">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="663c2-117">Vygenerované uživatelské rozhraní MovieController</span><span class="sxs-lookup"><span data-stu-id="663c2-117">Scaffold the MovieController</span></span>

<span data-ttu-id="663c2-118">Otevřete okno terminálu ve složce projektu a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="663c2-118">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> <span data-ttu-id="663c2-119">Pokud dojde k chybě při spuštění příkazu generování uživatelského rozhraní, přečtěte si téma [vydání 444 v úložišti generování uživatelského rozhraní](https://github.com/aspnet/scaffolding/issues/444) alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="663c2-119">If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.</span></span>

<span data-ttu-id="663c2-120">Modul generování uživatelského rozhraní vytvoří následující:</span><span class="sxs-lookup"><span data-stu-id="663c2-120">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="663c2-121">Řadič filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="663c2-121">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="663c2-122">Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="663c2-122">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="663c2-123">Automatické vytváření [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*.</span><span class="sxs-lookup"><span data-stu-id="663c2-123">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="663c2-124">Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.</span><span class="sxs-lookup"><span data-stu-id="663c2-124">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="663c2-125">Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data.</span><span class="sxs-lookup"><span data-stu-id="663c2-125">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="663c2-126">V dalším kurzu jsme budete pracovat s databází.</span><span class="sxs-lookup"><span data-stu-id="663c2-126">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="663c2-127">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="663c2-127">Additional resources</span></span>

* [<span data-ttu-id="663c2-128">Pomocníci značky</span><span class="sxs-lookup"><span data-stu-id="663c2-128">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="663c2-129">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="663c2-129">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="663c2-130">[Předchozí - přidat zobrazení](adding-view.md)
[další – práce s SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="663c2-130">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
