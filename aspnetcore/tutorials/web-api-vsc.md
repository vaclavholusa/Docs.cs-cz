---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio Code
author: rick-anderson
description: Vytvoření webového rozhraní API v systému macOS, Linux nebo Windows pomocí ASP.NET Core MVC a Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: e549bc3adf3efa32b3ac975cf04a35f508a554d5
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325624"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="f494a-103">Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f494a-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="f494a-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f494a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f494a-105">V tomto kurzu se vytvoření webového rozhraní API pro správu seznam "úkolů".</span><span class="sxs-lookup"><span data-stu-id="f494a-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="f494a-106">Není vytvořen uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f494a-106">A UI isn't constructed.</span></span>

<span data-ttu-id="f494a-107">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="f494a-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="f494a-108">macOS, Linux, Windows: webového rozhraní API pomocí Visual Studio Code (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="f494a-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="f494a-109">macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="f494a-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="f494a-110">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="f494a-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="f494a-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f494a-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="f494a-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="f494a-112">Create the project</span></span>

<span data-ttu-id="f494a-113">Z konzoly spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f494a-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="f494a-114">*TodoApi* otevře složka ve Visual Studio Code (VS Code).</span><span class="sxs-lookup"><span data-stu-id="f494a-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="f494a-115">Vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="f494a-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="f494a-116">Vyberte **Ano** k **upozornit** zpráva "požadované prostředky pro vytvoření a odladění chybí"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="f494a-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="f494a-117">Přidáním?"</span><span class="sxs-lookup"><span data-stu-id="f494a-117">Add them?"</span></span>
* <span data-ttu-id="f494a-118">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="f494a-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code s upozornit požadované prostředky pro sestavování a ladění 'TodoApi' chybí.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="f494a-122">Stisknutím klávesy **ladění** (F5) a sestavte a spusťte program.</span><span class="sxs-lookup"><span data-stu-id="f494a-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="f494a-123">V prohlížeči přejděte na http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="f494a-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="f494a-124">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="f494a-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="f494a-125">Zobrazit [nápovědy Visual Studio Code](#visual-studio-code-help) tipy pro používání VS Code.</span><span class="sxs-lookup"><span data-stu-id="f494a-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="f494a-126">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f494a-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f494a-127">Vytvoření nového projektu ASP.NET Core 2.1 nebo novější přidá [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) odkaz na balíček *TodoApi.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="f494a-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f494a-128">Vytvoření nového projektu v aplikaci ASP.NET Core 2.0 přidá [metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) odkaz na balíček *TodoApi.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="f494a-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="f494a-129">Není nutné k instalaci [Entity Framework Core InMemory](/ef/core/providers/in-memory/) databázi zprostředkovatele samostatně.</span><span class="sxs-lookup"><span data-stu-id="f494a-129">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="f494a-130">Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s databází v paměti.</span><span class="sxs-lookup"><span data-stu-id="f494a-130">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="f494a-131">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="f494a-131">Add a model class</span></span>

<span data-ttu-id="f494a-132">Model je objekt, který reprezentuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f494a-132">A model is an object representing the data in your app.</span></span> <span data-ttu-id="f494a-133">V tomto případě jediný model je položky úkolu.</span><span class="sxs-lookup"><span data-stu-id="f494a-133">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="f494a-134">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="f494a-134">Add a folder named *Models*.</span></span> <span data-ttu-id="f494a-135">Třídy modelu můžete umístit kdekoli ve vašem projektu, ale *modely* složky používají konvence.</span><span class="sxs-lookup"><span data-stu-id="f494a-135">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="f494a-136">Přidat `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f494a-136">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f494a-137">Vytvoří databázi `Id` při `TodoItem` se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="f494a-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="f494a-138">Vytvořte kontext databáze</span><span class="sxs-lookup"><span data-stu-id="f494a-138">Create the database context</span></span>

<span data-ttu-id="f494a-139">*Kontext databáze* je hlavní třída, která koordinuje funkce Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="f494a-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="f494a-140">Vytvoření této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="f494a-140">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="f494a-141">Přidat `TodoContext` třídy v *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="f494a-141">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="f494a-142">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="f494a-142">Add a controller</span></span>

<span data-ttu-id="f494a-143">V *řadiče* složku, vytvořte třídu s názvem `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f494a-143">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="f494a-144">Nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f494a-144">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="f494a-145">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="f494a-145">Launch the app</span></span>

<span data-ttu-id="f494a-146">V nástroji VS Code stisknutím klávesy F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f494a-146">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="f494a-147">Přejděte do http://localhost:5000/api/todo ( `Todo` řadič jsme vytvořili).</span><span class="sxs-lookup"><span data-stu-id="f494a-147">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="f494a-148">Visual Studio Code – Nápověda</span><span class="sxs-lookup"><span data-stu-id="f494a-148">Visual Studio Code help</span></span>

* [<span data-ttu-id="f494a-149">Začínáme</span><span class="sxs-lookup"><span data-stu-id="f494a-149">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="f494a-150">Ladění</span><span class="sxs-lookup"><span data-stu-id="f494a-150">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="f494a-151">Integrovaný terminál</span><span class="sxs-lookup"><span data-stu-id="f494a-151">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="f494a-152">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="f494a-152">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="f494a-153">macOS klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="f494a-153">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="f494a-154">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="f494a-154">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="f494a-155">Windows klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="f494a-155">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
