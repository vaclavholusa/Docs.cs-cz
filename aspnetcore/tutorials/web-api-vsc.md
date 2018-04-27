---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio Code
author: rick-anderson
description: Sestavení webového rozhraní API v systému macOS, Linux nebo Windows s ASP.NET MVC jádra a Visual Studio Code
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 83a6a09f8c0fd399efeb38903786bc41ff8ac4fb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="e0cdb-103">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e0cdb-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="e0cdb-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e0cdb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e0cdb-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="e0cdb-106">Uživatelského rozhraní není vytvořená.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-106">A UI isn't constructed.</span></span>

<span data-ttu-id="e0cdb-107">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="e0cdb-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="e0cdb-108">systému macOS, Linux, Windows: webového rozhraní API s Visual Studio Code (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="e0cdb-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="e0cdb-109">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="e0cdb-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="e0cdb-110">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="e0cdb-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE [template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="e0cdb-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e0cdb-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="e0cdb-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="e0cdb-112">Create the project</span></span>

<span data-ttu-id="e0cdb-113">Z konzoly spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="e0cdb-113">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="e0cdb-114">Otevřete *TodoApi* složky v kódu pro Visual Studio (VS kódu) a vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-114">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="e0cdb-115">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-115">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="e0cdb-116">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="e0cdb-116">Add them?"</span></span>
- <span data-ttu-id="e0cdb-117">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="e0cdb-117">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' chybí VS Code s varování požadované prostředky pro sestavení a ladění.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="e0cdb-121">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-121">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="e0cdb-122">V prohlížeči přejděte na http://localhost:5000/api/values .</span><span class="sxs-lookup"><span data-stu-id="e0cdb-122">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="e0cdb-123">Zobrazí se:</span><span class="sxs-lookup"><span data-stu-id="e0cdb-123">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="e0cdb-124">V tématu [Visual Studio Code nápovědy](#visual-studio-code-help) tipy pro používání VS Code.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-124">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="e0cdb-125">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e0cdb-125">Add support for Entity Framework Core</span></span>

<span data-ttu-id="e0cdb-126">Vytvoření nového projektu v rozhraní .NET 2.0 základní přidá poskytovatele 'Microsoft.AspNetCore.All' v *TodoApi.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-126">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="e0cdb-127">Není nutné k instalaci [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) databáze zprostředkovatele samostatně.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-127">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="e0cdb-128">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="e0cdb-129">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="e0cdb-129">Add a model class</span></span>

<span data-ttu-id="e0cdb-130">Model je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-130">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="e0cdb-131">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-131">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="e0cdb-132">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-132">Add a folder named *Models*.</span></span> <span data-ttu-id="e0cdb-133">Třídy modelu můžete umístit kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-133">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="e0cdb-134">Přidat `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e0cdb-134">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e0cdb-135">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-135">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="e0cdb-136">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="e0cdb-136">Create the database context</span></span>

<span data-ttu-id="e0cdb-137">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-137">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="e0cdb-138">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-138">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="e0cdb-139">Přidat `TodoContext` třídy v *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="e0cdb-139">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE [Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="e0cdb-140">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="e0cdb-140">Add a controller</span></span>

<span data-ttu-id="e0cdb-141">V *řadiče* složky, vytvořte třídu s názvem `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-141">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="e0cdb-142">Přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="e0cdb-142">Add the following code:</span></span>

[!INCLUDE [code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="e0cdb-143">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="e0cdb-143">Launch the app</span></span>

<span data-ttu-id="e0cdb-144">V produktu VS Code stisknutím klávesy F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e0cdb-144">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="e0cdb-145">Přejděte na http://localhost:5000/api/todo ( `Todo` řadiče jsme právě vytvořili).</span><span class="sxs-lookup"><span data-stu-id="e0cdb-145">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[Javascript Jquery](../includes/add-javascript-jquery/index.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="e0cdb-146">Visual Studio Code nápovědy</span><span class="sxs-lookup"><span data-stu-id="e0cdb-146">Visual Studio Code help</span></span>

- [<span data-ttu-id="e0cdb-147">Začínáme</span><span class="sxs-lookup"><span data-stu-id="e0cdb-147">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="e0cdb-148">Ladění</span><span class="sxs-lookup"><span data-stu-id="e0cdb-148">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="e0cdb-149">Integrované terminálu</span><span class="sxs-lookup"><span data-stu-id="e0cdb-149">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="e0cdb-150">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="e0cdb-150">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="e0cdb-151">systému macOS klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="e0cdb-151">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="e0cdb-152">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="e0cdb-152">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="e0cdb-153">Klávesové zkratky systému Windows</span><span class="sxs-lookup"><span data-stu-id="e0cdb-153">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE [next steps](../includes/webApi/next.md)]

