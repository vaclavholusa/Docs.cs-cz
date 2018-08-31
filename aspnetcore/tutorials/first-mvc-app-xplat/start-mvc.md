---
title: Úvod do ASP.NET Core MVC v systému macOS, Linux nebo Windows
author: rick-anderson
description: Zjistěte, jak začít pracovat s ASP.NET Core MVC a Visual Studio Code na macOS, Linux a Windows
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/31/2018
ms.locfileid: "36275272"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a><span data-ttu-id="0416c-103">Úvod do ASP.NET Core MVC v systému macOS, Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="0416c-103">Introduction to ASP.NET Core MVC on macOS, Linux, or Windows</span></span>

<span data-ttu-id="0416c-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0416c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0416c-105">V tomto kurzu se seznámíte se základy vytváření webové aplikace ASP.NET Core MVC pomocí [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="0416c-105">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="0416c-106">Kurz předpokládá familarity s VS Code.</span><span class="sxs-lookup"><span data-stu-id="0416c-106">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="0416c-107">Zobrazit [Začínáme s VS Code](https://code.visualstudio.com/docs) a [nápovědy Visual Studio Code](#visual-studio-code-help) Další informace.</span><span class="sxs-lookup"><span data-stu-id="0416c-107">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="0416c-108">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="0416c-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="0416c-109">macOS: [vytvoření aplikace ASP.NET Core MVC se sadou Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="0416c-109">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="0416c-110">Windows: [vytvořit v aplikaci MVC ASP.NET Core pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="0416c-110">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="0416c-111">macOS, Linux a Windows: [vytvoření aplikace ASP.NET Core MVC pomocí Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="0416c-111">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="0416c-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="0416c-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="0416c-113">Vytvoření webové aplikace s dotnet</span><span class="sxs-lookup"><span data-stu-id="0416c-113">Create a web app with dotnet</span></span>

<span data-ttu-id="0416c-114">Z terminálu spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0416c-114">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="0416c-115">Otevřít *MvcMovie* složky ve Visual Studio Code (VS Code) a vyberte *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="0416c-115">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="0416c-116">Vyberte **Ano** k **upozornit** zpráva "požadované prostředky pro vytvoření a odladění chybí"MvcMovie".</span><span class="sxs-lookup"><span data-stu-id="0416c-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="0416c-117">Přidáním?"</span><span class="sxs-lookup"><span data-stu-id="0416c-117">Add them?"</span></span>
- <span data-ttu-id="0416c-118">Vyberte **obnovení** k **informace** zpráva "Neexistují nevyřešené závislosti".</span><span class="sxs-lookup"><span data-stu-id="0416c-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code s upozornit požadované prostředky pro sestavování a ladění 'MvcMovie' chybí.](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="0416c-122">Stisknutím klávesy **ladění** (F5) a sestavte a spusťte program.</span><span class="sxs-lookup"><span data-stu-id="0416c-122">Press **Debug** (F5) to build and run the program.</span></span>

![spuštění aplikace](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="0416c-124">Spuštění VS Code [Kestrel](xref:fundamentals/servers/kestrel) webového serveru vaší aplikace a spustí.</span><span class="sxs-lookup"><span data-stu-id="0416c-124">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="0416c-125">Všimněte si, že do adresního řádku ukazuje `localhost:5000` a nemít něco podobného `example.com`.</span><span class="sxs-lookup"><span data-stu-id="0416c-125">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="0416c-126">Důvodem je, že `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="0416c-126">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="0416c-127">Výchozí šablony vám práci **domácí o** a **kontakt** odkazy.</span><span class="sxs-lookup"><span data-stu-id="0416c-127">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="0416c-128">Výše uvedené prohlížeče obraz se nezobrazí těchto odkazů.</span><span class="sxs-lookup"><span data-stu-id="0416c-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="0416c-129">V závislosti na velikosti vašeho prohlížeče mohou muset kliknout na ikonu navigace se budou zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="0416c-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Ikona navigace v vpravo nahoře](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="0416c-131">V další části tohoto kurzu přidáme další informace o MVC a začít psát kód.</span><span class="sxs-lookup"><span data-stu-id="0416c-131">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="0416c-132">Visual Studio Code – Nápověda</span><span class="sxs-lookup"><span data-stu-id="0416c-132">Visual Studio Code help</span></span>

- [<span data-ttu-id="0416c-133">Začínáme</span><span class="sxs-lookup"><span data-stu-id="0416c-133">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="0416c-134">Ladění</span><span class="sxs-lookup"><span data-stu-id="0416c-134">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="0416c-135">Integrovaný terminál</span><span class="sxs-lookup"><span data-stu-id="0416c-135">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="0416c-136">Klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="0416c-136">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="0416c-137">macOS klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="0416c-137">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="0416c-138">Linux klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="0416c-138">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="0416c-139">Windows klávesové zkratky</span><span class="sxs-lookup"><span data-stu-id="0416c-139">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

> [!div class="step-by-step"]
> [<span data-ttu-id="0416c-140">Další – Přidání kontroleru</span><span class="sxs-lookup"><span data-stu-id="0416c-140">Next - Add a controller</span></span>](adding-controller.md)
