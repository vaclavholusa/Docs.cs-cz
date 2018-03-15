---
title: "Úvod do základní ASP.NET MVC na Mac, Linux nebo Windows"
author: rick-anderson
description: "Zjistěte, jak začít pracovat s ASP.NET MVC jádra a Visual Studio Code na Mac, Linux a Windows"
manager: wpickett
ms.author: riande
ms.date: 07/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 4960f5b2a82d20b9325f870296f4385e14e15b37
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
---
# <a name="introduction-to-aspnet-core-mvc-on-mac-linux-or-windows"></a><span data-ttu-id="a6c3d-103">Úvod do základní ASP.NET MVC na Mac, Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="a6c3d-103">Introduction to ASP.NET Core MVC on Mac, Linux, or Windows</span></span>

<span data-ttu-id="a6c3d-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a6c3d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a6c3d-105">V tomto kurzu naučit, základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="a6c3d-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="a6c3d-106">Kurz předpokládá familarity kódem VS.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="a6c3d-107">V tématu [Začínáme s VS Code](https://code.visualstudio.com/docs) a [Visual Studio Code nápovědy](#visual-studio-code-help) Další informace.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="a6c3d-108">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="a6c3d-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a6c3d-109">systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a6c3d-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="a6c3d-110">Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a6c3d-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="a6c3d-111">systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="a6c3d-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="a6c3d-112">Instalace VS Code a .NET Core</span><span class="sxs-lookup"><span data-stu-id="a6c3d-112">Install VS Code and .NET Core</span></span>

<span data-ttu-id="a6c3d-113">Tento kurz vyžaduje [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-113">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="a6c3d-114">V tématu [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) pro verzi ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-114">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="a6c3d-115">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="a6c3d-115">Install the following:</span></span>

* <span data-ttu-id="a6c3d-116">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-116">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="a6c3d-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6c3d-117">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="a6c3d-118">VS Code [rozšíření C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="a6c3d-118">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="a6c3d-119">Vytvoření webové aplikace s dotnet.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-119">Create a web app with dotnet</span></span>

<span data-ttu-id="a6c3d-120">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="a6c3d-120">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="a6c3d-121">Otevřete *MvcMovie* složky v kódu pro Visual Studio (VS kódu) a vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-121">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="a6c3d-122">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-122">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="a6c3d-123">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="a6c3d-123">Add them?"</span></span>
- <span data-ttu-id="a6c3d-124">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="a6c3d-124">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

!['MvcMovie' chybí VS Code s varování požadované prostředky pro sestavení a ladění.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="a6c3d-128">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-128">Press **Debug** (F5) to build and run the program.</span></span>

![spuštění aplikace](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="a6c3d-130">VS kód spustí [Kestrel](xref:fundamentals/servers/kestrel) webový server a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-130">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="a6c3d-131">Všimněte si, že se zobrazí na panelu Adresa `localhost:5000` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-131">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="a6c3d-132">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-132">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="a6c3d-133">Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-133">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="a6c3d-134">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-134">The browser image above doesn't show these links.</span></span> <span data-ttu-id="a6c3d-135">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-135">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![v pravé horní části ikonu Navigace](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="a6c3d-137">V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="a6c3d-137">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="a6c3d-138">Visual Studio Code nápovědy</span><span class="sxs-lookup"><span data-stu-id="a6c3d-138">Visual Studio Code help</span></span>

- [<span data-ttu-id="a6c3d-139">Začínáme</span><span class="sxs-lookup"><span data-stu-id="a6c3d-139">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="a6c3d-140">Ladění</span><span class="sxs-lookup"><span data-stu-id="a6c3d-140">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="a6c3d-141">Integrované terminálu</span><span class="sxs-lookup"><span data-stu-id="a6c3d-141">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="a6c3d-142">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="a6c3d-142">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="a6c3d-143">Mac klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="a6c3d-143">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="a6c3d-144">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="a6c3d-144">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="a6c3d-145">Klávesové zkratky systému Windows</span><span class="sxs-lookup"><span data-stu-id="a6c3d-145">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="a6c3d-146">Další – Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="a6c3d-146">Next - Add a controller</span></span>](adding-controller.md)
