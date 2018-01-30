---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows"
author: rick-anderson
description: "Sestavení webového rozhraní API s ASP.NET MVC jádra a Visual Studio pro Windows"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1146132693681eca8f92beb00ebabd7296534688
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="a7cd6-103">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows</span><span class="sxs-lookup"><span data-stu-id="a7cd6-103">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="a7cd6-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a7cd6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a7cd6-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="a7cd6-106">Uživatelské rozhraní (UI) se nevytvoří.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="a7cd6-107">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="a7cd6-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a7cd6-108">Windows: Webové rozhraní API s Visual Studio pro Windows (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="a7cd6-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="a7cd6-109">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="a7cd6-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="a7cd6-110">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="a7cd6-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="a7cd6-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a7cd6-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="a7cd6-112">V tématu [tento PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) pro verzi ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-112">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a7cd6-113">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="a7cd6-113">Create the project</span></span>

<span data-ttu-id="a7cd6-114">Ze sady Visual Studio, vyberte **soubor** nabídce > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-114">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="a7cd6-115">Vyberte **webové aplikace ASP.NET Core (.NET Core)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-115">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="a7cd6-116">Název projektu `TodoApi` a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-116">Name the project `TodoApi` and select **OK**.</span></span>

![Dialogové okno Nový projekt](first-web-api/_static/new-project.png)

<span data-ttu-id="a7cd6-118">V **nové základní webové aplikace ASP.NET - TodoApi** dialogovém okně, vyberte **webového rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="a7cd6-119">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-119">Select **OK**.</span></span> <span data-ttu-id="a7cd6-120">Proveďte **není** vyberte **povolení podpory Docker**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-120">Do **not** select **Enable Docker Support**.</span></span>

![Dialogové okno nové webové aplikace ASP.NET pomocí šablony projektu webového rozhraní API vybrané ze šablon jádro ASP.NET](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="a7cd6-122">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="a7cd6-122">Launch the app</span></span>

<span data-ttu-id="a7cd6-123">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="a7cd6-124">Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-124">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="a7cd6-125">Chrome, Microsoft Edge a Firefox zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="a7cd6-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="a7cd6-126">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="a7cd6-126">Add a model class</span></span>

<span data-ttu-id="a7cd6-127">Model je objekt, který představuje data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-127">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="a7cd6-128">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-128">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="a7cd6-129">Přidáte složku s názvem "Modely".</span><span class="sxs-lookup"><span data-stu-id="a7cd6-129">Add a folder named "Models".</span></span> <span data-ttu-id="a7cd6-130">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="a7cd6-131">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a7cd6-132">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-132">Name the folder *Models*.</span></span>

<span data-ttu-id="a7cd6-133">Poznámka: Třídy modelu přejděte kdekoli v projektu.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-133">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="a7cd6-134">*Modely* složky používají konvence pro třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="a7cd6-135">Přidat `TodoItem` třídy.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="a7cd6-136">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="a7cd6-137">Název třídy `TodoItem` a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="a7cd6-138">Aktualizace `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="a7cd6-138">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a7cd6-139">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="a7cd6-140">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="a7cd6-140">Create the database context</span></span>

<span data-ttu-id="a7cd6-141">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a7cd6-142">Tato třída se vytvoří odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="a7cd6-143">Přidat `TodoContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="a7cd6-144">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="a7cd6-145">Název třídy `TodoContext` a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="a7cd6-146">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="a7cd6-146">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="a7cd6-147">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="a7cd6-147">Add a controller</span></span>

<span data-ttu-id="a7cd6-148">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="a7cd6-149">Vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="a7cd6-150">V **přidat novou položku** dialogovém okně, vyberte **třída Kontroleru rozhraní Web API** šablony.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-150">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="a7cd6-151">Název třídy `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-151">Name the class `TodoController`.</span></span>

![Přidání nové položky dialogové okno s řadiče v vyhledávací pole a webové rozhraní API řadič vybrané](first-web-api/_static/new_controller.png)

<span data-ttu-id="a7cd6-153">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="a7cd6-153">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="a7cd6-154">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="a7cd6-154">Launch the app</span></span>

<span data-ttu-id="a7cd6-155">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="a7cd6-156">Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="a7cd6-157">Přejděte na `Todo` ovladač na `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="a7cd6-157">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

