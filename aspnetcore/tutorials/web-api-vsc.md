---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a VS Code"
author: rick-anderson
description: "Sestavení webového rozhraní API v systému macOS, Linux nebo Windows s ASP.NET MVC jádra a Visual Studio Code"
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 12b1c3cea5101b4da673a1ad82cc1ad461f2a38d
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/16/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="64811-103">Vytvoření webového rozhraní API s ASP.NET MVC jádra a Visual Studio Code v systému Windows, Linux a systému macOS</span><span class="sxs-lookup"><span data-stu-id="64811-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="64811-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="64811-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="64811-105">V tomto kurzu sestavení webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="64811-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="64811-106">Uživatelského rozhraní není vytvořená.</span><span class="sxs-lookup"><span data-stu-id="64811-106">A UI isn't constructed.</span></span>

<span data-ttu-id="64811-107">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="64811-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="64811-108">systému macOS, Linux, Windows: webového rozhraní API s Visual Studio Code (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="64811-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="64811-109">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="64811-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="64811-110">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="64811-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="64811-111">Nastavení vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="64811-111">Set up your development environment</span></span>

<span data-ttu-id="64811-112">Stáhněte a nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="64811-112">Download and install:</span></span>
- <span data-ttu-id="64811-113">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="64811-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="64811-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="64811-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="64811-115">Visual Studio Code [rozšíření C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="64811-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="64811-116">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="64811-116">Create the project</span></span>

<span data-ttu-id="64811-117">Z konzoly spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="64811-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="64811-118">Otevřete *TodoApi* složky v kódu pro Visual Studio (VS kódu) a vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="64811-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="64811-119">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="64811-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="64811-120">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="64811-120">Add them?"</span></span>
- <span data-ttu-id="64811-121">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="64811-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' chybí VS Code s varování požadované prostředky pro sestavení a ladění.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="64811-125">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="64811-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="64811-126">V prohlížeči přejděte na http://localhost: 5000/api/hodnoty.</span><span class="sxs-lookup"><span data-stu-id="64811-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="64811-127">Zobrazí se:</span><span class="sxs-lookup"><span data-stu-id="64811-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="64811-128">V tématu [Visual Studio Code nápovědy](#visual-studio-code-help) tipy pro používání VS Code.</span><span class="sxs-lookup"><span data-stu-id="64811-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="64811-129">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="64811-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="64811-130">Vytvoření nového projektu v rozhraní .NET 2.0 základní přidá poskytovatele 'Microsoft.AspNetCore.All' v *TodoApi.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="64811-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="64811-131">Není nutné k instalaci [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) databáze zprostředkovatele samostatně.</span><span class="sxs-lookup"><span data-stu-id="64811-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="64811-132">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="64811-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="64811-133">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="64811-133">Add a model class</span></span>

<span data-ttu-id="64811-134">Model je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="64811-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="64811-135">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="64811-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="64811-136">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="64811-136">Add a folder named *Models*.</span></span> <span data-ttu-id="64811-137">Třídy modelu můžete umístit kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="64811-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="64811-138">Přidat `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="64811-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="64811-139">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="64811-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="64811-140">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="64811-140">Create the database context</span></span>

<span data-ttu-id="64811-141">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="64811-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="64811-142">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="64811-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="64811-143">Přidat `TodoContext` třídy v *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="64811-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="64811-144">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="64811-144">Add a controller</span></span>

<span data-ttu-id="64811-145">V *řadiče* složky, vytvořte třídu s názvem `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="64811-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="64811-146">Přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="64811-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="64811-147">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="64811-147">Launch the app</span></span>

<span data-ttu-id="64811-148">V produktu VS Code stisknutím klávesy F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="64811-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="64811-149">Přejděte na http://localhost: 5000/api/todo ( `Todo` řadiče jsme právě vytvořili).</span><span class="sxs-lookup"><span data-stu-id="64811-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="64811-150">Visual Studio Code nápovědy</span><span class="sxs-lookup"><span data-stu-id="64811-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="64811-151">Začínáme</span><span class="sxs-lookup"><span data-stu-id="64811-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="64811-152">Ladění</span><span class="sxs-lookup"><span data-stu-id="64811-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="64811-153">Integrované terminálu</span><span class="sxs-lookup"><span data-stu-id="64811-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="64811-154">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="64811-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="64811-155">systému macOS klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="64811-155">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="64811-156">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="64811-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="64811-157">Klávesové zkratky systému Windows</span><span class="sxs-lookup"><span data-stu-id="64811-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


