---
title: Začínáme s ASP.NET MVC jádra a sady Visual Studio pro Mac
author: rick-anderson
description: Zjistěte, jak začít pracovat s ASP.NET MVC jádra a sady Visual Studio
ms.author: riande
ms.date: 8/23/2017
uid: tutorials/first-mvc-app-mac/start-mvc
ms.openlocfilehash: e94b9aa6b6c594ae407792387788410f776d4c1d
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272290"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="45b44-103">Začínáme s ASP.NET MVC jádra a sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="45b44-103">Get started with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="45b44-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="45b44-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="45b44-105">V tomto kurzu se dozvíte, jaké základní informace o vytváření ASP.NET MVC základní webové aplikace pomocí [Visual Studio pro Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span><span class="sxs-lookup"><span data-stu-id="45b44-105">This tutorial teaches you the basics of building an ASP.NET Core MVC web app using [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/).</span></span> 

[!INCLUDE [consider RP](../../includes/razor.md)]

<span data-ttu-id="45b44-106">Existují 3 verze tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="45b44-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="45b44-107">systému macOS: [sestavení aplikace ASP.NET MVC základní pomocí sady Visual Studio pro Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="45b44-107">macOS: [Build an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="45b44-108">Windows: [sestavení ASP.NET Core aplikace MVC pomocí sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="45b44-108">Windows: [Build an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="45b44-109">Systému macOS, Linux a Windows: [sestavení aplikace ASP.NET MVC jádra s kódem jazyka Visual Studio](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="45b44-109">Linux, macOS, and Windows: [Build an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45b44-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="45b44-110">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-web-app"></a><span data-ttu-id="45b44-111">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="45b44-111">Create a web app</span></span>

<span data-ttu-id="45b44-112">Ze sady Visual Studio, vyberte **soubor > Nový řešení**.</span><span class="sxs-lookup"><span data-stu-id="45b44-112">From Visual Studio, select **File > New Solution**.</span></span>

![systému macOS nové řešení](../first-web-api-mac/_static/sln.png)

<span data-ttu-id="45b44-114">Vyberte **aplikace .NET Core > ASP.NET Core > Webová aplikace > Další**.</span><span class="sxs-lookup"><span data-stu-id="45b44-114">Select **.NET Core App >  ASP.NET Core > Web App > Next**.</span></span>

![systému macOS dialogové okno Nový projekt](start-mvc/1.png)

<span data-ttu-id="45b44-116">Název projektu **MvcMovie**a potom vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="45b44-116">Name the project **MvcMovie**, and then select **Create**.</span></span>

![systému macOS dialogové okno Nový projekt](start-mvc/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="45b44-118">Spusťte aplikaci</span><span class="sxs-lookup"><span data-stu-id="45b44-118">Launch the app</span></span>

<span data-ttu-id="45b44-119">V sadě Visual Studio, vyberte **spustit > Spustit bez ladění** spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="45b44-119">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="45b44-120">Visual Studio spustí [Kestrel](xref:fundamentals/servers/index#kestrel), spustí se prohlížeč a přejde na `http://localhost:port`, kde *portu* je náhodně vybraný port číslo.</span><span class="sxs-lookup"><span data-stu-id="45b44-120">Visual Studio starts [Kestrel](xref:fundamentals/servers/index#kestrel), launches a browser, and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span>

![Prohlížeč s nový projekt](start-mvc/b1.png)

* <span data-ttu-id="45b44-122">Zobrazí panel Adresa `localhost:port#` a není něco jako `example.com`.</span><span class="sxs-lookup"><span data-stu-id="45b44-122">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="45b44-123">Je to způsobeno `localhost` je standardní název hostitele místního počítače.</span><span class="sxs-lookup"><span data-stu-id="45b44-123">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="45b44-124">Když Visual Studio vytvoří webový projekt, náhodný port se používá pro webový server.</span><span class="sxs-lookup"><span data-stu-id="45b44-124">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="45b44-125">Při spuštění aplikace se zobrazí jiné číslo portu.</span><span class="sxs-lookup"><span data-stu-id="45b44-125">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="45b44-126">Můžete spustit aplikaci v ladění nebo režim bez ladění z **spustit** nabídky.</span><span class="sxs-lookup"><span data-stu-id="45b44-126">You can launch the app in debug or non-debug mode from the **Run** menu.</span></span>

<span data-ttu-id="45b44-127">Výchozí šablony vám **domácí o** a **kontaktujte** odkazy.</span><span class="sxs-lookup"><span data-stu-id="45b44-127">The default template gives you **Home, About** and **Contact** links.</span></span> <span data-ttu-id="45b44-128">Na obrázku prohlížeče výše nezobrazí tyto odkazy.</span><span class="sxs-lookup"><span data-stu-id="45b44-128">The browser image above doesn't show these links.</span></span> <span data-ttu-id="45b44-129">V závislosti na velikosti prohlížeče možná budete muset klikněte na ikonu navigace je chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="45b44-129">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Prohlížeč s nový projekt](start-mvc/b2.png)

<span data-ttu-id="45b44-131">V další části tohoto kurzu Další informace o MVC a zahájit zápis nějaký kód.</span><span class="sxs-lookup"><span data-stu-id="45b44-131">In the next part of this tutorial, you learn about MVC and start writing some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="45b44-132">Next</span><span class="sxs-lookup"><span data-stu-id="45b44-132">Next</span></span>](adding-controller.md)  
