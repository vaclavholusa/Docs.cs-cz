---
title: "Úvod do základní ASP.NET MVC na Mac, Linux nebo Windows"
author: rick-anderson
description: "Začínáme s ASP.NET MVC jádra a Visual Studio Code na Mac, Linux a Windows"
keywords: "Jádro ASP.NET, MVC, VS kódu, Visual Studio Code, Mac, Linux, Windows"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="4efa6-104">Začínáme s ASP.NET MVC jádra na Mac, Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="4efa6-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="4efa6-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4efa6-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4efa6-106">V tomto kurzu naučit, základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="4efa6-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="4efa6-107">Kurz předpokládá familarity kódem VS.</span><span class="sxs-lookup"><span data-stu-id="4efa6-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="4efa6-108">V tématu [Začínáme s VS Code](https://code.visualstudio.com/docs) a [Visual Studio Code nápovědy](#visual-studio-code-help) Další informace.</span><span class="sxs-lookup"><span data-stu-id="4efa6-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="4efa6-109">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="4efa6-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="4efa6-110">systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4efa6-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="4efa6-111">Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4efa6-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="4efa6-112">systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="4efa6-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="4efa6-113">Instalace VS Code a .NET Core</span><span class="sxs-lookup"><span data-stu-id="4efa6-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="4efa6-114">Tento kurz vyžaduje [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4efa6-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="4efa6-115">V tématu [pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) pro verzi ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="4efa6-115">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="4efa6-116">Nainstalujte následující:</span><span class="sxs-lookup"><span data-stu-id="4efa6-116">Install the following:</span></span>

* <span data-ttu-id="4efa6-117">[.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4efa6-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="4efa6-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4efa6-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="4efa6-119">VS Code [rozšíření C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="4efa6-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="4efa6-120">Vytvoření webové aplikace s dotnet.</span><span class="sxs-lookup"><span data-stu-id="4efa6-120">Create a web app with dotnet</span></span>

<span data-ttu-id="4efa6-121">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="4efa6-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="4efa6-122">Otevřete *MvcMovie* složky v kódu pro Visual Studio (VS kódu) a vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="4efa6-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="4efa6-123">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="4efa6-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="4efa6-124">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="4efa6-124">Add them?"</span></span>
- <span data-ttu-id="4efa6-125">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="4efa6-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

!['MvcMovie' chybí VS Code s varování požadované prostředky pro sestavení a ladění.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="4efa6-129">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="4efa6-129">Press **Debug** (F5) to build and run the program.</span></span>

![spuštění aplikace](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="4efa6-131">VS kód spustí [Kestrel](xref:fundamentals/servers/kestrel) webový server a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="4efa6-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="4efa6-132">Všimněte si, že se zobrazí na panelu Adresa `localhost:5000` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="4efa6-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="4efa6-133">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="4efa6-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="4efa6-134">Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="4efa6-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="4efa6-135">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="4efa6-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="4efa6-136">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="4efa6-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![v pravé horní části ikonu Navigace](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="4efa6-138">V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="4efa6-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="4efa6-139">Visual Studio Code nápovědy</span><span class="sxs-lookup"><span data-stu-id="4efa6-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="4efa6-140">Začínáme</span><span class="sxs-lookup"><span data-stu-id="4efa6-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="4efa6-141">Ladění</span><span class="sxs-lookup"><span data-stu-id="4efa6-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="4efa6-142">Integrované terminálu</span><span class="sxs-lookup"><span data-stu-id="4efa6-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="4efa6-143">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="4efa6-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="4efa6-144">Mac klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="4efa6-144">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="4efa6-145">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="4efa6-145">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="4efa6-146">Klávesové zkratky systému Windows</span><span class="sxs-lookup"><span data-stu-id="4efa6-146">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="4efa6-147">Další – Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="4efa6-147">Next - Add a controller</span></span>](adding-controller.md)
