---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows
author: rick-anderson
description: Sestavení webového rozhraní API s ASP.NET MVC jádra a Visual Studio pro Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 3da22cbbbe0db7771656997a13587521e182fb2a
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/25/2018
ms.locfileid: "36277397"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="649fa-103">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows</span><span class="sxs-lookup"><span data-stu-id="649fa-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="649fa-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="649fa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="649fa-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="649fa-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="649fa-106">Uživatelské rozhraní (UI) se nevytvoří.</span><span class="sxs-lookup"><span data-stu-id="649fa-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="649fa-107">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="649fa-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="649fa-108">Windows: Webové rozhraní API s Visual Studio pro Windows (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="649fa-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="649fa-109">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="649fa-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="649fa-110">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="649fa-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="649fa-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="649fa-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="649fa-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="649fa-112">Create the project</span></span>

<span data-ttu-id="649fa-113">Pomocí těchto kroků v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="649fa-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="649fa-114">Z **soubor** nabídce vyberte možnost **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="649fa-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="649fa-115">Vyberte **webové aplikace ASP.NET Core** šablony.</span><span class="sxs-lookup"><span data-stu-id="649fa-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="649fa-116">Název projektu *TodoApi* a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="649fa-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="649fa-117">V **nové základní webové aplikace ASP.NET - TodoApi** dialogovém okně, vyberte verzi ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="649fa-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="649fa-118">Vyberte **rozhraní API** šablonu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="649fa-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="649fa-119">Proveďte **není** vyberte **povolení podpory Docker**.</span><span class="sxs-lookup"><span data-stu-id="649fa-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="649fa-120">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="649fa-120">Launch the app</span></span>

<span data-ttu-id="649fa-121">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="649fa-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="649fa-122">Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="649fa-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="649fa-123">Chrome, Microsoft Edge a Firefox zobrazí následující výstup:</span><span class="sxs-lookup"><span data-stu-id="649fa-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="649fa-124">Pokud používáte Internet Explorer, budete vyzváni k uložení *values.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="649fa-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="649fa-125">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="649fa-125">Add a model class</span></span>

<span data-ttu-id="649fa-126">Model je objekt reprezentující data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="649fa-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="649fa-127">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="649fa-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="649fa-128">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="649fa-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="649fa-129">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="649fa-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="649fa-130">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="649fa-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="649fa-131">Třídy modelu můžete přejít kdekoli v projektu.</span><span class="sxs-lookup"><span data-stu-id="649fa-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="649fa-132">*Modely* složky používají konvence pro třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="649fa-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="649fa-133">V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="649fa-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="649fa-134">Název třídy *TodoItem* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="649fa-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="649fa-135">Aktualizace `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="649fa-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="649fa-136">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="649fa-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="649fa-137">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="649fa-137">Create the database context</span></span>

<span data-ttu-id="649fa-138">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="649fa-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="649fa-139">Tato třída se vytvoří odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="649fa-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="649fa-140">V Průzkumníku řešení klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="649fa-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="649fa-141">Název třídy *TodoContext* a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="649fa-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="649fa-142">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="649fa-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="649fa-143">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="649fa-143">Add a controller</span></span>

<span data-ttu-id="649fa-144">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="649fa-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="649fa-145">Vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="649fa-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="649fa-146">V **přidat novou položku** dialogovém okně, vyberte **třída Kontroleru rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="649fa-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="649fa-147">Název třídy *TodoController*a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="649fa-147">Name the class *TodoController*, and click **Add**.</span></span>

![Přidání nové položky dialogové okno s řadiče v vyhledávací pole a webové rozhraní API řadič vybrané](first-web-api/_static/new_controller.png)

<span data-ttu-id="649fa-149">Třída nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="649fa-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="649fa-150">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="649fa-150">Launch the app</span></span>

<span data-ttu-id="649fa-151">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="649fa-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="649fa-152">Visual Studio spustí prohlížeč a přejde na `http://localhost:<port>/api/values`, kde `<port>` je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="649fa-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="649fa-153">Přejděte na `Todo` ovladač na `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="649fa-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
