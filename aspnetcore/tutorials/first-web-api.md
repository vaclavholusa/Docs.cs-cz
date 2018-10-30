---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio
author: rick-anderson
description: Vytvoření webového rozhraní API pomocí ASP.NET Core MVC a sady Visual Studio ve Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 88d1958ce5c42d559754972a855c1ffe22ab45a6
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234576"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="74a9d-103">Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74a9d-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="74a9d-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="74a9d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="74a9d-105">V tomto kurzu sestavení webového rozhraní API pro správu seznam "úkolů".</span><span class="sxs-lookup"><span data-stu-id="74a9d-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="74a9d-106">Uživatelské rozhraní (UI) není vytvořena.</span><span class="sxs-lookup"><span data-stu-id="74a9d-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="74a9d-107">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="74a9d-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="74a9d-108">Windows: Webové rozhraní API pomocí sady Visual Studio na Windows (kurz, najdete v článku [videoverzi](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="74a9d-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="74a9d-109">macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="74a9d-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="74a9d-110">macOS, Linux, Windows: [webového rozhraní API pomocí Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="74a9d-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="74a9d-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="74a9d-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="74a9d-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="74a9d-112">Create the project</span></span>

<span data-ttu-id="74a9d-113">Postupujte podle těchto kroků v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="74a9d-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="74a9d-114">Z **souboru** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="74a9d-115">Vyberte **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="74a9d-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="74a9d-116">Pojmenujte projekt *TodoApi* a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="74a9d-117">V **nové webové aplikace ASP.NET Core – TodoApi** dialogového okna, vyberte verzi technologie ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="74a9d-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="74a9d-118">Vyberte **API** šablonu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="74a9d-119">Proveďte **není** vyberte **povolit podporu Dockeru**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="74a9d-120">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="74a9d-120">Launch the app</span></span>

<span data-ttu-id="74a9d-121">V sadě Visual Studio stiskněte CTRL + F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74a9d-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="74a9d-122">Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je číslo portu náhodně vybrané.</span><span class="sxs-lookup"><span data-stu-id="74a9d-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="74a9d-123">Chrome, Microsoft Edge a Firefox se zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="74a9d-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="74a9d-124">Pokud používáte Internet Explorer, budete vyzváni k uložení *values.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="74a9d-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="74a9d-125">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="74a9d-125">Add a model class</span></span>

<span data-ttu-id="74a9d-126">Model je objekt reprezentující data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74a9d-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="74a9d-127">V tomto případě jediný model je položky úkolu.</span><span class="sxs-lookup"><span data-stu-id="74a9d-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="74a9d-128">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="74a9d-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="74a9d-129">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="74a9d-130">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="74a9d-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="74a9d-131">Třídy modelu můžete kamkoliv v projektu.</span><span class="sxs-lookup"><span data-stu-id="74a9d-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="74a9d-132">*Modely* složky používají konvence pro třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="74a9d-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="74a9d-133">V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* a pak zvolte položku **přidat** > **třídy**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="74a9d-134">Název třídy *TodoItem* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="74a9d-135">Aktualizace `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="74a9d-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="74a9d-136">Vytvoří databázi `Id` při `TodoItem` se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="74a9d-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="74a9d-137">Vytvořte kontext databáze</span><span class="sxs-lookup"><span data-stu-id="74a9d-137">Create the database context</span></span>

<span data-ttu-id="74a9d-138">*Kontext databáze* je hlavní třída, která koordinuje funkce Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="74a9d-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="74a9d-139">Tato třída se vytvoří odvozením z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="74a9d-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="74a9d-140">V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* a pak zvolte položku **přidat** > **třídy**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="74a9d-141">Název třídy *TodoContext* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="74a9d-142">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="74a9d-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="74a9d-143">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="74a9d-143">Add a controller</span></span>

<span data-ttu-id="74a9d-144">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="74a9d-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="74a9d-145">Vyberte **přidat** > **nová položka**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="74a9d-146">V **přidat novou položku** dialogového okna, vyberte **třída Kontroleru rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="74a9d-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="74a9d-147">Název třídy *TodoController*a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="74a9d-147">Name the class *TodoController*, and click **Add**.</span></span>

![Přidání nové položky dialogové okno s kontrolerem v vyhledávací pole a webové rozhraní API kontroleru vybrané](first-web-api/_static/new_controller.png)

<span data-ttu-id="74a9d-149">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="74a9d-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="74a9d-150">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="74a9d-150">Launch the app</span></span>

<span data-ttu-id="74a9d-151">V sadě Visual Studio stiskněte CTRL + F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="74a9d-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="74a9d-152">Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je číslo portu náhodně vybrané.</span><span class="sxs-lookup"><span data-stu-id="74a9d-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="74a9d-153">Přejděte `Todo` ovladač na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="74a9d-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
