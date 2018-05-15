---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows
author: rick-anderson
description: Sestavení webového rozhraní API s ASP.NET MVC jádra a Visual Studio pro Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: cb46f8b4013488dbe2bb5ca3d08a8c6e452141dd
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="495f0-103">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows</span><span class="sxs-lookup"><span data-stu-id="495f0-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="495f0-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="495f0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="495f0-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="495f0-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="495f0-106">Uživatelské rozhraní (UI) se nevytvoří.</span><span class="sxs-lookup"><span data-stu-id="495f0-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="495f0-107">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="495f0-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="495f0-108">Windows: Webové rozhraní API s Visual Studio pro Windows (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="495f0-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="495f0-109">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="495f0-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="495f0-110">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="495f0-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="495f0-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="495f0-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="495f0-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="495f0-112">Create the project</span></span>

<span data-ttu-id="495f0-113">Pomocí těchto kroků v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="495f0-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="495f0-114">Z **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="495f0-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="495f0-115">Vyberte **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="495f0-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="495f0-116">Název projektu *TodoApi* a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="495f0-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="495f0-117">V **nové základní webové aplikace ASP.NET - TodoApi** dialogovém okně, vyberte verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="495f0-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="495f0-118">Vyberte **rozhraní API** šablonu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="495f0-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="495f0-119">Proveďte **není** vyberte **povolení podpory Docker**.</span><span class="sxs-lookup"><span data-stu-id="495f0-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="495f0-120">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="495f0-120">Launch the app</span></span>

<span data-ttu-id="495f0-121">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="495f0-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="495f0-122">Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="495f0-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="495f0-123">Chrome, Microsoft Edge a Firefox zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="495f0-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="495f0-124">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="495f0-124">Add a model class</span></span>

<span data-ttu-id="495f0-125">Model je objekt reprezentující data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="495f0-125">A model is an object representing the data in the app.</span></span> <span data-ttu-id="495f0-126">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="495f0-126">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="495f0-127">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="495f0-127">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="495f0-128">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="495f0-128">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="495f0-129">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="495f0-129">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="495f0-130">Třídy modelu můžete přejít kdekoli v projektu.</span><span class="sxs-lookup"><span data-stu-id="495f0-130">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="495f0-131">*Modely* složky používají konvence pro třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="495f0-131">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="495f0-132">V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="495f0-132">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="495f0-133">Název třídy *TodoItem* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="495f0-133">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="495f0-134">Aktualizace `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="495f0-134">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="495f0-135">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="495f0-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="495f0-136">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="495f0-136">Create the database context</span></span>

<span data-ttu-id="495f0-137">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="495f0-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="495f0-138">Tato třída se vytvoří odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="495f0-138">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="495f0-139">V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="495f0-139">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="495f0-140">Název třídy *TodoContext* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="495f0-140">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="495f0-141">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="495f0-141">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="495f0-142">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="495f0-142">Add a controller</span></span>

<span data-ttu-id="495f0-143">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="495f0-143">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="495f0-144">Vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="495f0-144">Select **Add** > **New Item**.</span></span> <span data-ttu-id="495f0-145">V **přidat novou položku** dialogovém okně, vyberte **třída Kontroleru rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="495f0-145">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="495f0-146">Název třídy *TodoController*a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="495f0-146">Name the class *TodoController*, and click **Add**.</span></span>

![Přidání nové položky dialogové okno s řadiče v vyhledávací pole a webové rozhraní API řadič vybrané](first-web-api/_static/new_controller.png)

<span data-ttu-id="495f0-148">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="495f0-148">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="495f0-149">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="495f0-149">Launch the app</span></span>

<span data-ttu-id="495f0-150">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="495f0-150">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="495f0-151">Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="495f0-151">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="495f0-152">Přejděte na `Todo` ovladač na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="495f0-152">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
