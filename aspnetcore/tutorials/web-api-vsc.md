---
title: "Vytvoření webové rozhraní API pomocí ASP.NET Core a VS Code"
description: "Sestavení webového rozhraní API v systému macOS, Linux nebo Windows s ASP.NET MVC jádra a Visual Studio Code"
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: "ASP.NET Core, WebAPI, webové rozhraní API, REST, Mac, Linux, HTTP, služby, služba HTTP, VS Code"
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: caf40ee1c2d45d2fbf33b07d707fa4f1be98d31c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="d8125-104">Vytvoření webového rozhraní API s ASP.NET MVC jádra a Visual Studio Code v systému Windows, Linux a systému macOS</span><span class="sxs-lookup"><span data-stu-id="d8125-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="d8125-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Wasson Jan](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="d8125-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="d8125-106">V tomto kurzu vytvoříte webového rozhraní API pro správu "úkolů" položek seznamu.</span><span class="sxs-lookup"><span data-stu-id="d8125-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="d8125-107">Nebude sestavení uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d8125-107">You won’t build a UI.</span></span>

<span data-ttu-id="d8125-108">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="d8125-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="d8125-109">systému macOS, Linux, Windows: webového rozhraní API s Visual Studio Code (Tento kurz)</span><span class="sxs-lookup"><span data-stu-id="d8125-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="d8125-110">systému macOS: [webového rozhraní API pomocí sady Visual Studio pro Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="d8125-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="d8125-111">Windows: [webové rozhraní API pomocí sady Visual Studio pro Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="d8125-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="d8125-112">Nastavení vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="d8125-112">Set up your development environment</span></span>

<span data-ttu-id="d8125-113">Stáhněte a nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="d8125-113">Download and install:</span></span>
- <span data-ttu-id="d8125-114">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d8125-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="d8125-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="d8125-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="d8125-116">Visual Studio Code [rozšíření C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="d8125-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="d8125-117">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="d8125-117">Create the project</span></span>

<span data-ttu-id="d8125-118">Z konzoly spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="d8125-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="d8125-119">Otevřete *TodoApi* složky v kódu pro Visual Studio (VS kódu) a vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="d8125-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="d8125-120">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'TodoApi'.</span><span class="sxs-lookup"><span data-stu-id="d8125-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="d8125-121">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="d8125-121">Add them?"</span></span>
- <span data-ttu-id="d8125-122">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="d8125-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

!['TodoApi' chybí VS Code s varování požadované prostředky pro sestavení a ladění.](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="d8125-126">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="d8125-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="d8125-127">V prohlížeči přejděte na http://localhost: 5000/api/hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d8125-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="d8125-128">Zobrazí se:</span><span class="sxs-lookup"><span data-stu-id="d8125-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="d8125-129">V tématu [Visual Studio Code nápovědy](#visual-studio-code-help) tipy pro používání VS Code.</span><span class="sxs-lookup"><span data-stu-id="d8125-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="d8125-130">Přidání podpory pro Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d8125-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="d8125-131">Upravit *TodoApi.csproj* k instalaci [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) zprostředkovatel databáze.</span><span class="sxs-lookup"><span data-stu-id="d8125-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="d8125-132">Tento poskytovatel databáze umožňuje Entity Framework Core pro použití s databázi v paměti.</span><span class="sxs-lookup"><span data-stu-id="d8125-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

<span data-ttu-id="d8125-133">Spustit `dotnet restore` ke stažení a instalaci zprostředkovatele EF DB InMemory jádra.</span><span class="sxs-lookup"><span data-stu-id="d8125-133">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="d8125-134">Můžete spustit `dotnet restore` z terminálu nebo zadejte `⌘⇧P` (macOS) nebo `Ctrl+Shift+P` (Linux) v VS Code a pak zadejte **.NET**.</span><span class="sxs-lookup"><span data-stu-id="d8125-134">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="d8125-135">Vyberte **.NET: obnovení balíčků**.</span><span class="sxs-lookup"><span data-stu-id="d8125-135">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="d8125-136">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="d8125-136">Add a model class</span></span>

<span data-ttu-id="d8125-137">Model je objekt, který představuje data ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d8125-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="d8125-138">V takovém případě je pouze model položku seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="d8125-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="d8125-139">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="d8125-139">Add a folder named *Models*.</span></span> <span data-ttu-id="d8125-140">Třídy modelu můžete umístit kdekoli v projektu, ale *modely* složky se používá podle konvence.</span><span class="sxs-lookup"><span data-stu-id="d8125-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="d8125-141">Přidat `TodoItem` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="d8125-141">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="d8125-142">Generuje databázi `Id` při `TodoItem` je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="d8125-142">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="d8125-143">Vytvoření kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="d8125-143">Create the database context</span></span>

<span data-ttu-id="d8125-144">*Kontext databáze* je hlavní třída, která koordinuje funkcí rozhraní Entity Framework pro daný datový model.</span><span class="sxs-lookup"><span data-stu-id="d8125-144">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="d8125-145">Vytvořit této třídy odvozené z `Microsoft.EntityFrameworkCore.DbContext` třídy.</span><span class="sxs-lookup"><span data-stu-id="d8125-145">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="d8125-146">Přidat `TodoContext` třídy v *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="d8125-146">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="d8125-147">Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="d8125-147">Add a controller</span></span>

<span data-ttu-id="d8125-148">V *řadiče* složky, vytvořte třídu s názvem `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="d8125-148">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="d8125-149">Přidejte následující kód:</span><span class="sxs-lookup"><span data-stu-id="d8125-149">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="d8125-150">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="d8125-150">Launch the app</span></span>

<span data-ttu-id="d8125-151">V produktu VS Code stisknutím klávesy F5 spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d8125-151">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="d8125-152">Přejděte na http://localhost: 5000/api/todo ( `Todo` řadiče jsme právě vytvořili).</span><span class="sxs-lookup"><span data-stu-id="d8125-152">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="d8125-153">Visual Studio Code nápovědy</span><span class="sxs-lookup"><span data-stu-id="d8125-153">Visual Studio Code help</span></span>

- [<span data-ttu-id="d8125-154">Začínáme</span><span class="sxs-lookup"><span data-stu-id="d8125-154">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="d8125-155">Ladění</span><span class="sxs-lookup"><span data-stu-id="d8125-155">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="d8125-156">Integrované terminálu</span><span class="sxs-lookup"><span data-stu-id="d8125-156">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="d8125-157">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="d8125-157">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="d8125-158">Mac klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="d8125-158">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="d8125-159">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="d8125-159">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="d8125-160">Klávesové zkratky systému Windows</span><span class="sxs-lookup"><span data-stu-id="d8125-160">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


