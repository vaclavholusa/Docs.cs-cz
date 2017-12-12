---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows"
author: rick-anderson
description: "Sestavení webového rozhraní API s ASP.NET MVC jádra a Visual Studio pro Windows"
keywords: "Jádro ASP.NET WebAPI, webového rozhraní API, REST, HTTP, služby, služba HTTP"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: da47296fd952300ce60121603834a9f22be3c339
ms.sourcegitcommit: 703593d5fd14076e79be2ba75a5b8da12a60ab15
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="91d1b-104">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows</span><span class="sxs-lookup"><span data-stu-id="91d1b-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="91d1b-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="91d1b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="91d1b-106">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="91d1b-106">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="91d1b-107">Uživatelské rozhraní (UI) se nevytvoří.</span><span class="sxs-lookup"><span data-stu-id="91d1b-107">A user interface (UI) is not created.</span></span>

<span data-ttu-id="91d1b-108">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="91d1b-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="91d1b-109">Windows: Webové rozhraní API s Visual Studio pro Windows (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="91d1b-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="91d1b-110">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="91d1b-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="91d1b-111">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="91d1b-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="91d1b-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="91d1b-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="91d1b-113">V tématu [tento PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) pro verzi ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="91d1b-113">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="91d1b-114">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="91d1b-114">Create the project</span></span>

<span data-ttu-id="91d1b-115">Ze sady Visual Studio, vyberte **soubor** nabídce > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="91d1b-116">Vyberte **webové aplikace ASP.NET Core (.NET Core)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="91d1b-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="91d1b-117">Název projektu `TodoApi` a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-117">Name the project `TodoApi` and select **OK**.</span></span>

![Dialogové okno Nový projekt](first-web-api/_static/new-project.png)

<span data-ttu-id="91d1b-119">V **nové základní webové aplikace ASP.NET - TodoApi** dialogovém okně, vyberte **webového rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="91d1b-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="91d1b-120">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-120">Select **OK**.</span></span> <span data-ttu-id="91d1b-121">Proveďte **není** vyberte **povolení podpory Docker**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-121">Do **not** select **Enable Docker Support**.</span></span>

![Dialogové okno nové webové aplikace ASP.NET pomocí šablony projektu webového rozhraní API vybrané ze šablon jádro ASP.NET](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="91d1b-123">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="91d1b-123">Launch the app</span></span>

<span data-ttu-id="91d1b-124">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91d1b-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="91d1b-125">Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="91d1b-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="91d1b-126">Chrome, Microsoft Edge a Firefox zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="91d1b-126">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="91d1b-127">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="91d1b-127">Add a model class</span></span>

<span data-ttu-id="91d1b-128">Model je objekt, který představuje data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91d1b-128">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="91d1b-129">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="91d1b-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="91d1b-130">Přidáte složku s názvem "Modely".</span><span class="sxs-lookup"><span data-stu-id="91d1b-130">Add a folder named "Models".</span></span> <span data-ttu-id="91d1b-131">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="91d1b-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="91d1b-132">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="91d1b-133">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="91d1b-133">Name the folder *Models*.</span></span>

<span data-ttu-id="91d1b-134">Poznámka: Třídy modelu přejděte kdekoli v projektu.</span><span class="sxs-lookup"><span data-stu-id="91d1b-134">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="91d1b-135">*Modely* složky používají konvence pro třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="91d1b-135">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="91d1b-136">Přidat `TodoItem` třídy.</span><span class="sxs-lookup"><span data-stu-id="91d1b-136">Add a `TodoItem` class.</span></span> <span data-ttu-id="91d1b-137">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-137">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="91d1b-138">Název třídy `TodoItem` a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-138">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="91d1b-139">Aktualizace `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="91d1b-139">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="91d1b-140">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="91d1b-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="91d1b-141">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="91d1b-141">Create the database context</span></span>

<span data-ttu-id="91d1b-142">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="91d1b-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="91d1b-143">Tato třída se vytvoří odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="91d1b-143">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="91d1b-144">Přidat `TodoContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="91d1b-144">Add a `TodoContext` class.</span></span> <span data-ttu-id="91d1b-145">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-145">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="91d1b-146">Název třídy `TodoContext` a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-146">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="91d1b-147">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="91d1b-147">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="91d1b-148">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="91d1b-148">Add a controller</span></span>

<span data-ttu-id="91d1b-149">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="91d1b-149">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="91d1b-150">Vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="91d1b-150">Select **Add** > **New Item**.</span></span> <span data-ttu-id="91d1b-151">V **přidat novou položku** dialogovém okně, vyberte **třída Kontroleru rozhraní Web API** šablony.</span><span class="sxs-lookup"><span data-stu-id="91d1b-151">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="91d1b-152">Název třídy `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="91d1b-152">Name the class `TodoController`.</span></span>

![Přidání nové položky dialogové okno s řadiče v vyhledávací pole a webové rozhraní API řadič vybrané](first-web-api/_static/new_controller.png)

<span data-ttu-id="91d1b-154">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="91d1b-154">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="91d1b-155">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="91d1b-155">Launch the app</span></span>

<span data-ttu-id="91d1b-156">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="91d1b-156">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="91d1b-157">Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="91d1b-157">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="91d1b-158">Přejděte na `Todo` ovladač na `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="91d1b-158">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

