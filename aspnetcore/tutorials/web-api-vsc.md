---
title: Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio Code
author: rick-anderson
description: Sestavení webového rozhraní API v systému macOS, Linux nebo Windows s ASP.NET MVC jádra a Visual Studio Code
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 303016c7e1d27726faaa0b1546575255355d5275
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="1e720-103">Vytvoření webové rozhraní API pomocí ASP.NET Core a Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1e720-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="1e720-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="1e720-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="1e720-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="1e720-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="1e720-106">Uživatelského rozhraní není vytvořená.</span><span class="sxs-lookup"><span data-stu-id="1e720-106">A UI isn't constructed.</span></span>

<span data-ttu-id="1e720-107">Existují tři verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="1e720-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="1e720-108">systému macOS, Linux, Windows: webového rozhraní API s Visual Studio Code (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="1e720-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="1e720-109">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="1e720-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="1e720-110">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="1e720-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="1e720-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1e720-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="1e720-112">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="1e720-112">Create the project</span></span>

<span data-ttu-id="1e720-113">Z konzoly spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1e720-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="1e720-114">*TodoApi* složky otevře ve Visual Studio Code (kód VS).</span><span class="sxs-lookup"><span data-stu-id="1e720-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="1e720-115">Vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="1e720-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="1e720-116">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="1e720-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="1e720-117">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="1e720-117">Add them?"</span></span>
* <span data-ttu-id="1e720-118">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="1e720-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' chybí VS Code s varování požadované prostředky pro sestavení a ladění.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="1e720-122">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="1e720-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="1e720-123">V prohlížeči přejděte na http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="1e720-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="1e720-124">Zobrazí se následující výstup:</span><span class="sxs-lookup"><span data-stu-id="1e720-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="1e720-125">V tématu [Visual Studio Code nápovědy](#visual-studio-code-help) tipy pro používání VS Code.</span><span class="sxs-lookup"><span data-stu-id="1e720-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="1e720-126">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1e720-126">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="1e720-127">Vytvoření nového projektu v technologii ASP.NET 2.0 základní přidá [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) odkaz na balíček *TodoApi.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="1e720-127">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="1e720-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="1e720-128">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="1e720-129">Vytvoření nového projektu ASP.NET Core 2.1 nebo novější přidá [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) odkaz na balíček *TodoApi.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="1e720-129">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="1e720-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="1e720-130">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="1e720-131">Není nutné k instalaci [Entity Framework Core InMemory](/ef/core/providers/in-memory/) databáze zprostředkovatele samostatně.</span><span class="sxs-lookup"><span data-stu-id="1e720-131">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="1e720-132">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="1e720-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="1e720-133">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="1e720-133">Add a model class</span></span>

<span data-ttu-id="1e720-134">Model je objekt reprezentující data v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1e720-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="1e720-135">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="1e720-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="1e720-136">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="1e720-136">Add a folder named *Models*.</span></span> <span data-ttu-id="1e720-137">Třídy modelu můžete umístit kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="1e720-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="1e720-138">Přidat `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1e720-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="1e720-139">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="1e720-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="1e720-140">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="1e720-140">Create the database context</span></span>

<span data-ttu-id="1e720-141">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="1e720-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="1e720-142">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="1e720-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="1e720-143">Přidat `TodoContext` třídy v *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="1e720-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="1e720-144">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="1e720-144">Add a controller</span></span>

<span data-ttu-id="1e720-145">V *řadiče* složky, vytvořte třídu s názvem `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="1e720-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="1e720-146">Nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1e720-146">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="1e720-147">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="1e720-147">Launch the app</span></span>

<span data-ttu-id="1e720-148">V produktu VS Code stisknutím klávesy F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1e720-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="1e720-149">Přejděte na http://localhost:5000/api/todo ( `Todo` řadiče jsme vytvořili).</span><span class="sxs-lookup"><span data-stu-id="1e720-149">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="1e720-150">Visual Studio Code nápovědy</span><span class="sxs-lookup"><span data-stu-id="1e720-150">Visual Studio Code help</span></span>

* [<span data-ttu-id="1e720-151">Začínáme</span><span class="sxs-lookup"><span data-stu-id="1e720-151">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="1e720-152">Ladění</span><span class="sxs-lookup"><span data-stu-id="1e720-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="1e720-153">Integrované terminálu</span><span class="sxs-lookup"><span data-stu-id="1e720-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="1e720-154">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="1e720-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="1e720-155">systému macOS klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="1e720-155">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="1e720-156">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="1e720-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="1e720-157">Klávesové zkratky systému Windows</span><span class="sxs-lookup"><span data-stu-id="1e720-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
