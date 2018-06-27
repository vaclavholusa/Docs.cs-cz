---
title: Přidat model do aplikace ASP.NET MVC jádra
author: rick-anderson
description: Přidáte model do jednoduchou aplikaci ASP.NET Core.
ms.author: riande
ms.date: 09/18/2017
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 82b8338f10cb4d58ae06bdb70583e1563c2e6b64
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961422"
---
[!INCLUDE [adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="928a9-103">Přidání třídy k *modely* složku s názvem *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="928a9-103">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="928a9-104">Přidejte následující kód, který *Models/Movie.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="928a9-104">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="928a9-105">`ID` Pole je vyžadováno pro databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="928a9-105">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="928a9-106">Vytvořit aplikaci pro ověření nemáte žádné chyby a nakonec jste přidali **M**odelu k vaší **M**VC aplikace.</span><span class="sxs-lookup"><span data-stu-id="928a9-106">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="928a9-107">Příprava projektu pro generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="928a9-107">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="928a9-108">Přidejte následující zvýrazněnou balíčků NuGet *MvcMovie.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="928a9-108">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="928a9-109">Uložte tento soubor a vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="928a9-109">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="928a9-110">Vytvoření *Models/MvcMovieContext.cs* souboru a přidejte následující `MvcMovieContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="928a9-110">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="928a9-111">Otevřete *Startup.cs* souboru a přidejte dva direktiv Using:</span><span class="sxs-lookup"><span data-stu-id="928a9-111">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="928a9-112">Přidat kontext databáze, který má *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="928a9-112">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="928a9-113">Tato hodnota informuje Entity Framework, které třídy modelu jsou zahrnuty v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="928a9-113">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="928a9-114">Definujete jeden *sady entit* film objektů, které bude možné vyjádřit v databázi jako film tabulku.</span><span class="sxs-lookup"><span data-stu-id="928a9-114">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="928a9-115">Sestavte projekt a ověřte, zda že nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="928a9-115">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="928a9-116">Vygenerované uživatelské rozhraní MovieController</span><span class="sxs-lookup"><span data-stu-id="928a9-116">Scaffold the MovieController</span></span>

<span data-ttu-id="928a9-117">Otevřete okno terminálu ve složce projektu a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="928a9-117">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="928a9-118">Modul generování uživatelského rozhraní vytvoří následující:</span><span class="sxs-lookup"><span data-stu-id="928a9-118">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="928a9-119">Řadič filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="928a9-119">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="928a9-120">Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="928a9-120">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="928a9-121">Automatické vytváření [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*.</span><span class="sxs-lookup"><span data-stu-id="928a9-121">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="928a9-122">Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.</span><span class="sxs-lookup"><span data-stu-id="928a9-122">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="928a9-123">Nyní máte databázi a stránkami zobrazit, upravit, aktualizovat a odstranit data.</span><span class="sxs-lookup"><span data-stu-id="928a9-123">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="928a9-124">V dalším kurzu jsme budete pracovat s databází.</span><span class="sxs-lookup"><span data-stu-id="928a9-124">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="928a9-125">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="928a9-125">Additional resources</span></span>

* [<span data-ttu-id="928a9-126">Pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="928a9-126">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="928a9-127">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="928a9-127">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="928a9-128">[Předchozí - přidat zobrazení](adding-view.md)
> [další – práce s SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="928a9-128">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
