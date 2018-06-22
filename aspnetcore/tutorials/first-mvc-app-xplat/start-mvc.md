---
title: Úvod do architektury ASP.NET MVC jádra v systému macOS, Linux nebo Windows
author: rick-anderson
description: Zjistěte, jak začít pracovat s ASP.NET MVC jádra a Visual Studio Code v systému macOS, Linux a Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275272"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="cfbf0-103">Úvod do architektury ASP.NET MVC jádra v systému macOS, Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="cfbf0-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="cfbf0-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cfbf0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cfbf0-105">V tomto kurzu naučit, základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="cfbf0-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="cfbf0-106">Kurz předpokládá familarity kódem VS.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="cfbf0-107">V tématu [Začínáme s VS Code](https://code.visualstudio.com/docs) a [Visual Studio Code nápovědy](#visual-studio-code-help) Další informace.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="cfbf0-108">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="cfbf0-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="cfbf0-109">systému macOS: [vytvořit aplikaci ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="cfbf0-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="cfbf0-110">Windows: [vytvořit základní ASP.NET MVC aplikace pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="cfbf0-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="cfbf0-111">systému macOS, Linux a Windows: [vytvořit aplikaci ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="cfbf0-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cfbf0-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cfbf0-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="cfbf0-113">Vytvoření webové aplikace s dotnet.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-113">Create a web app with dotnet</span></span>

<span data-ttu-id="cfbf0-114">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="cfbf0-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="cfbf0-115">Otevřete *MvcMovie* složky v kódu pro Visual Studio (VS kódu) a vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="cfbf0-116">Vyberte **Ano** k **varování** zpráva "požadované prostředky pro sestavení a ladění chybí 'MvcMovie'.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="cfbf0-117">Přidat jejich?"</span><span class="sxs-lookup"><span data-stu-id="cfbf0-117">Add them?"</span></span>
- <span data-ttu-id="cfbf0-118">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="cfbf0-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

!['MvcMovie' chybí VS Code s varování požadované prostředky pro sestavení a ladění.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="cfbf0-122">Stiskněte klávesu **ladění** (F5) sestavení a spuštění programu.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-122">Press **Debug** (F5) to build and run the program.</span></span>

![spuštění aplikace](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="cfbf0-124">VS kód spustí [Kestrel](xref:fundamentals/servers/kestrel) webový server a spustí aplikace.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="cfbf0-125">Všimněte si, že se zobrazí na panelu Adresa `localhost:5000` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="cfbf0-126">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="cfbf0-127">Výchozí šablony vám práce **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="cfbf0-128">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="cfbf0-129">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![v pravé horní části ikonu Navigace](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="cfbf0-131">V další části tohoto kurzu jsme vám další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="cfbf0-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="cfbf0-132">Visual Studio Code nápovědy</span><span class="sxs-lookup"><span data-stu-id="cfbf0-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="cfbf0-133">Začínáme</span><span class="sxs-lookup"><span data-stu-id="cfbf0-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="cfbf0-134">Ladění</span><span class="sxs-lookup"><span data-stu-id="cfbf0-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="cfbf0-135">Integrované terminálu</span><span class="sxs-lookup"><span data-stu-id="cfbf0-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="cfbf0-136">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="cfbf0-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="cfbf0-137">systému macOS klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="cfbf0-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="cfbf0-138">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="cfbf0-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="cfbf0-139">Klávesové zkratky systému Windows</span><span class="sxs-lookup"><span data-stu-id="cfbf0-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="cfbf0-140">Další – Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="cfbf0-140">Next - Add a controller</span></span>](adding-controller.md)
