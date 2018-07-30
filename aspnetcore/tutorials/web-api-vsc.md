---
title: Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio Code
author: rick-anderson
description: Vytvoření webového rozhraní API v systému macOS, Linux nebo Windows pomocí ASP.NET Core MVC a Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4ce808ec4241ab2fc3c2fb81c3fdb15dd853cd90
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342273"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="83b1b-103">Vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="83b1b-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="83b1b-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="83b1b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="83b1b-105">V tomto kurzu se vytvoření webového rozhraní API pro správu seznam "úkolů".</span><span class="sxs-lookup"><span data-stu-id="83b1b-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="83b1b-106">Není vytvořen uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="83b1b-106">A UI isn't constructed.</span></span>

<span data-ttu-id="83b1b-107">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="83b1b-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="83b1b-108">macOS, Linux, Windows: webového rozhraní API pomocí Visual Studio Code (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="83b1b-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="83b1b-109">macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="83b1b-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="83b1b-110">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="83b1b-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="83b1b-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="83b1b-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="83b1b-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="83b1b-112">Create the project</span></span>

<span data-ttu-id="83b1b-113">Z konzoly spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="83b1b-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="83b1b-114">*TodoApi* otevře složka ve Visual Studio Code (VS Code).</span><span class="sxs-lookup"><span data-stu-id="83b1b-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="83b1b-115">Vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="83b1b-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="83b1b-116">Vyberte **Ano** k **upozornit** zpráva "požadované prostředky pro vytvoření a odladění chybí"TodoApi".</span><span class="sxs-lookup"><span data-stu-id="83b1b-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="83b1b-117">Přidáním?"</span><span class="sxs-lookup"><span data-stu-id="83b1b-117">Add them?"</span></span>
* <span data-ttu-id="83b1b-118">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="83b1b-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code s upozornit požadované prostředky pro sestavování a ladění 'TodoApi' chybí.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="83b1b-122">Stisknutím klávesy **ladění** (F5) a sestavte a spusťte program.</span><span class="sxs-lookup"><span data-stu-id="83b1b-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="83b1b-123">V prohlížeči přejděte na http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="83b1b-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="83b1b-124">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="83b1b-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="83b1b-125">Zobrazit [nápovědy Visual Studio Code](#visual-studio-code-help) tipy pro používání VS Code.</span><span class="sxs-lookup"><span data-stu-id="83b1b-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="83b1b-126">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="83b1b-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="83b1b-127">Vytvoření nového projektu ASP.NET Core 2.1 nebo novější přidá [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) odkaz na balíček *TodoApi.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="83b1b-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file.</span></span> <span data-ttu-id="83b1b-128">Přidat `Version` atribut, pokud ještě není zadán.</span><span class="sxs-lookup"><span data-stu-id="83b1b-128">Add the `Version` attribute, if not already specified.</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="83b1b-129">Vytvoření nového projektu v aplikaci ASP.NET Core 2.0 přidá [metabalíček](https://www.nuget.org/packages/Microsoft.AspNetCore.All) odkaz na balíček *TodoApi.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="83b1b-129">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="83b1b-130">Není nutné k instalaci [Entity Framework Core InMemory](/ef/core/providers/in-memory/) databázi zprostředkovatele samostatně.</span><span class="sxs-lookup"><span data-stu-id="83b1b-130">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="83b1b-131">Tento poskytovatel databáze umožňuje Entity Framework Core, který se má použít s databází v paměti.</span><span class="sxs-lookup"><span data-stu-id="83b1b-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="83b1b-132">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="83b1b-132">Add a model class</span></span>

<span data-ttu-id="83b1b-133">Model je objekt, který reprezentuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="83b1b-133">A model is an object representing the data in your app.</span></span> <span data-ttu-id="83b1b-134">V tomto případě jediný model je položky úkolu.</span><span class="sxs-lookup"><span data-stu-id="83b1b-134">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="83b1b-135">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="83b1b-135">Add a folder named *Models*.</span></span> <span data-ttu-id="83b1b-136">Třídy modelu můžete umístit kdekoli ve vašem projektu, ale *modely* složky používají konvence.</span><span class="sxs-lookup"><span data-stu-id="83b1b-136">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="83b1b-137">Přidat `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="83b1b-137">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="83b1b-138">Vytvoří databázi `Id` při `TodoItem` se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="83b1b-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="83b1b-139">Vytvořte kontext databáze</span><span class="sxs-lookup"><span data-stu-id="83b1b-139">Create the database context</span></span>

<span data-ttu-id="83b1b-140">*Kontext databáze* je hlavní třída, která koordinuje funkce Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="83b1b-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="83b1b-141">Vytvoření této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="83b1b-141">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="83b1b-142">Přidat `TodoContext` třídy v *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="83b1b-142">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="83b1b-143">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="83b1b-143">Add a controller</span></span>

<span data-ttu-id="83b1b-144">V *řadiče* složku, vytvořte třídu s názvem `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="83b1b-144">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="83b1b-145">Nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="83b1b-145">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="83b1b-146">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="83b1b-146">Launch the app</span></span>

<span data-ttu-id="83b1b-147">V nástroji VS Code stisknutím klávesy F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="83b1b-147">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="83b1b-148">Přejděte do http://localhost:5000/api/todo ( `Todo` řadič jsme vytvořili).</span><span class="sxs-lookup"><span data-stu-id="83b1b-148">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="83b1b-149">Visual Studio Code – Nápověda</span><span class="sxs-lookup"><span data-stu-id="83b1b-149">Visual Studio Code help</span></span>

* [<span data-ttu-id="83b1b-150">Začínáme</span><span class="sxs-lookup"><span data-stu-id="83b1b-150">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="83b1b-151">Ladění</span><span class="sxs-lookup"><span data-stu-id="83b1b-151">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="83b1b-152">Integrovaný terminál</span><span class="sxs-lookup"><span data-stu-id="83b1b-152">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="83b1b-153">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="83b1b-153">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="83b1b-154">macOS klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="83b1b-154">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="83b1b-155">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="83b1b-155">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="83b1b-156">Windows klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="83b1b-156">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
