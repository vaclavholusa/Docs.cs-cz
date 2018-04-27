---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows
author: rick-anderson
description: Sestavení webového rozhraní API s ASP.NET MVC jádra a Visual Studio pro Windows
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 92b1b28205584d2f08a5dc8124e5c50aa938c80f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="554ae-103">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows</span><span class="sxs-lookup"><span data-stu-id="554ae-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="554ae-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="554ae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="554ae-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="554ae-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="554ae-106">Uživatelské rozhraní (UI) se nevytvoří.</span><span class="sxs-lookup"><span data-stu-id="554ae-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="554ae-107">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="554ae-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="554ae-108">Windows: Webové rozhraní API s Visual Studio pro Windows (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="554ae-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="554ae-109">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="554ae-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="554ae-110">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="554ae-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="554ae-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="554ae-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="554ae-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="554ae-112">Create the project</span></span>

<span data-ttu-id="554ae-113">Ze sady Visual Studio, vyberte **soubor** nabídce > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="554ae-113">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="554ae-114">Vyberte **.NET Core** > **webové aplikace ASP.NET Core** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="554ae-114">Select **.NET Core** > **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="554ae-115">Název projektu `TodoApi` a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="554ae-115">Name the project `TodoApi` and select **OK**.</span></span>

![Dialogové okno Nový projekt](first-web-api/_static/new-project.png)

<span data-ttu-id="554ae-117">V **nové základní webové aplikace ASP.NET - TodoApi** dialogovém okně, vyberte **rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="554ae-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **API** template.</span></span> <span data-ttu-id="554ae-118">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="554ae-118">Select **OK**.</span></span> <span data-ttu-id="554ae-119">Proveďte **není** vyberte **povolení podpory Docker**.</span><span class="sxs-lookup"><span data-stu-id="554ae-119">Do **not** select **Enable Docker Support**.</span></span>

![Dialogové okno nové webové aplikace ASP.NET pomocí šablony projektu webového rozhraní API vybrané ze šablon jádro ASP.NET](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="554ae-121">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="554ae-121">Launch the app</span></span>

<span data-ttu-id="554ae-122">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="554ae-122">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="554ae-123">Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="554ae-123">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="554ae-124">Chrome, Microsoft Edge a Firefox zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="554ae-124">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="554ae-125">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="554ae-125">Add a model class</span></span>

<span data-ttu-id="554ae-126">Model je objekt, který představuje data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="554ae-126">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="554ae-127">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="554ae-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="554ae-128">Přidáte složku s názvem "Modely".</span><span class="sxs-lookup"><span data-stu-id="554ae-128">Add a folder named "Models".</span></span> <span data-ttu-id="554ae-129">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="554ae-129">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="554ae-130">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="554ae-130">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="554ae-131">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="554ae-131">Name the folder *Models*.</span></span>

<span data-ttu-id="554ae-132">Poznámka: Třídy modelu přejděte kdekoli v projektu.</span><span class="sxs-lookup"><span data-stu-id="554ae-132">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="554ae-133">*Modely* složky používají konvence pro třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="554ae-133">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="554ae-134">Přidat `TodoItem` třídy.</span><span class="sxs-lookup"><span data-stu-id="554ae-134">Add a `TodoItem` class.</span></span> <span data-ttu-id="554ae-135">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="554ae-135">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="554ae-136">Název třídy `TodoItem` a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="554ae-136">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="554ae-137">Aktualizace `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="554ae-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="554ae-138">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="554ae-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="554ae-139">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="554ae-139">Create the database context</span></span>

<span data-ttu-id="554ae-140">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="554ae-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="554ae-141">Tato třída se vytvoří odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="554ae-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="554ae-142">Přidat `TodoContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="554ae-142">Add a `TodoContext` class.</span></span> <span data-ttu-id="554ae-143">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="554ae-143">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="554ae-144">Název třídy `TodoContext` a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="554ae-144">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="554ae-145">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="554ae-145">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="554ae-146">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="554ae-146">Add a controller</span></span>

<span data-ttu-id="554ae-147">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="554ae-147">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="554ae-148">Vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="554ae-148">Select **Add** > **New Item**.</span></span> <span data-ttu-id="554ae-149">V **přidat novou položku** dialogovém okně, vyberte **třída Kontroleru rozhraní Web API** šablony.</span><span class="sxs-lookup"><span data-stu-id="554ae-149">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="554ae-150">Název třídy `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="554ae-150">Name the class `TodoController`.</span></span>

![Přidání nové položky dialogové okno s řadiče v vyhledávací pole a webové rozhraní API řadič vybrané](first-web-api/_static/new_controller.png)

<span data-ttu-id="554ae-152">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="554ae-152">Replace the class with the following code:</span></span>

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="554ae-153">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="554ae-153">Launch the app</span></span>

<span data-ttu-id="554ae-154">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="554ae-154">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="554ae-155">Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="554ae-155">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="554ae-156">Přejděte na `Todo` ovladač na `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="554ae-156">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE [last part of web API](../includes/webApi/end.md)]

[!INCLUDE[Javascript Jquery](../includes/add-javascript-jquery/index.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

