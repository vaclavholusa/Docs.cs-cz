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
ms.openlocfilehash: 617b11cd7652e393c06446c62138802e4a4e90df
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/19/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="e6d89-104">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows</span><span class="sxs-lookup"><span data-stu-id="e6d89-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="e6d89-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e6d89-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e6d89-106">V tomto kurzu vytvoříte webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="e6d89-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="e6d89-107">Nebude sestavení uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="e6d89-107">You won’t build a UI.</span></span>

<span data-ttu-id="e6d89-108">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="e6d89-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e6d89-109">Windows: Webové rozhraní API s Visual Studio pro Windows (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="e6d89-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="e6d89-110">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="e6d89-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="e6d89-111">systému macOS, Linux, Windows: [webového rozhraní API s kódem jazyka Visual Studio](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="e6d89-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="e6d89-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6d89-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="e6d89-113">V tématu [tento PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) pro verzi ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="e6d89-113">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="e6d89-114">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="e6d89-114">Create the project</span></span>

<span data-ttu-id="e6d89-115">Ze sady Visual Studio, vyberte **soubor** nabídce > **nový** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="e6d89-116">Vyberte **webové aplikace ASP.NET Core (.NET Core)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="e6d89-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="e6d89-117">Název projektu `TodoApi` a vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-117">Name the project `TodoApi` and select **OK**.</span></span>

![Dialogové okno Nový projekt](first-web-api/_static/new-project.png)

<span data-ttu-id="e6d89-119">V **nové základní webové aplikace ASP.NET - TodoApi** dialogovém okně, vyberte **webového rozhraní API** šablony.</span><span class="sxs-lookup"><span data-stu-id="e6d89-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="e6d89-120">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-120">Select **OK**.</span></span> <span data-ttu-id="e6d89-121">Proveďte **není** vyberte **povolení podpory Docker**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-121">Do **not** select **Enable Docker Support**.</span></span>

![Dialogové okno nové webové aplikace ASP.NET pomocí šablony projektu webového rozhraní API vybrané ze šablon jádro ASP.NET](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="e6d89-123">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="e6d89-123">Launch the app</span></span>

<span data-ttu-id="e6d89-124">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e6d89-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="e6d89-125">Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně zvolený port číslo.</span><span class="sxs-lookup"><span data-stu-id="e6d89-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="e6d89-126">Chrome, okraj a Firefox zobrazí následující:</span><span class="sxs-lookup"><span data-stu-id="e6d89-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="e6d89-127">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="e6d89-127">Add a model class</span></span>

<span data-ttu-id="e6d89-128">Model je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e6d89-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="e6d89-129">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="e6d89-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="e6d89-130">Přidáte složku s názvem "Modely".</span><span class="sxs-lookup"><span data-stu-id="e6d89-130">Add a folder named "Models".</span></span> <span data-ttu-id="e6d89-131">V Průzkumníku řešení klikněte pravým tlačítkem na projekt.</span><span class="sxs-lookup"><span data-stu-id="e6d89-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="e6d89-132">Vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e6d89-133">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="e6d89-133">Name the folder *Models*.</span></span>

<span data-ttu-id="e6d89-134">Poznámka: Třídy modelu go kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="e6d89-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="e6d89-135">Přidat `TodoItem` třídy.</span><span class="sxs-lookup"><span data-stu-id="e6d89-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="e6d89-136">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e6d89-137">Název třídy `TodoItem` a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="e6d89-138">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="e6d89-138">Replace the generated code with the following:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e6d89-139">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="e6d89-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="e6d89-140">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="e6d89-140">Create the database context</span></span>

<span data-ttu-id="e6d89-141">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="e6d89-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="e6d89-142">Tato třída se vytvoří odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="e6d89-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="e6d89-143">Přidat `TodoContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="e6d89-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="e6d89-144">Klikněte pravým tlačítkem myši *modely* složky a vyberte **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e6d89-145">Název třídy `TodoContext` a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="e6d89-146">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="e6d89-146">Replace the generated code with the following:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="e6d89-147">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="e6d89-147">Add a controller</span></span>

<span data-ttu-id="e6d89-148">V Průzkumníku řešení klikněte pravým tlačítkem myši *řadiče* složky.</span><span class="sxs-lookup"><span data-stu-id="e6d89-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="e6d89-149">Vyberte **přidat** > **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="e6d89-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="e6d89-150">V **přidat novou položku** dialogovém okně, vyberte **třída Kontroleru rozhraní Web API** šablony.</span><span class="sxs-lookup"><span data-stu-id="e6d89-150">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="e6d89-151">Název třídy `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e6d89-151">Name the class `TodoController`.</span></span>

![Přidání nové položky dialogové okno s řadiče v vyhledávací pole a webové rozhraní API řadič vybrané](first-web-api/_static/new_controller.png)

<span data-ttu-id="e6d89-153">Generovaného kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="e6d89-153">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="e6d89-154">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="e6d89-154">Launch the app</span></span>

<span data-ttu-id="e6d89-155">V sadě Visual Studio stiskněte CTRL + F5 a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e6d89-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="e6d89-156">Visual Studio spustí prohlížeč a přejde na `http://localhost:port/api/values`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="e6d89-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="e6d89-157">Pokud používáte Chrome, okraj nebo Firefox, zobrazí se data.</span><span class="sxs-lookup"><span data-stu-id="e6d89-157">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="e6d89-158">Pokud používáte IE, bude aplikace Internet Explorer výzva k můžete otevřít nebo Uložit *values.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="e6d89-158">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="e6d89-159">Přejděte na `Todo` řadiče jsme právě vytvořili `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="e6d89-159">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

