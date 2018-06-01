---
title: Začínáme s ASP.NET Core
author: rick-anderson
description: Rychlý kurz, který vytvoří a spustí jednoduchou aplikaci Hello World pomocí ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 1b4d765c08c67a65a8752e7f650d4e61ec0d2fbf
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687454"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="e6938-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6938-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="e6938-104">Nainstalujte [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="e6938-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="e6938-105">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6938-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="e6938-106">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="e6938-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e6938-107">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="e6938-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="e6938-108">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6938-108">Enter the following command:</span></span>

    ```terminal
    dotnet new webapp -o aspnetcoreapp
    ```

3. <span data-ttu-id="e6938-109">Spusťte aplikaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="e6938-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="e6938-110">Přejděte do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="e6938-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="e6938-111">Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="e6938-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="e6938-112">Je čas na serveru @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="e6938-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="e6938-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="e6938-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="e6938-114">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="e6938-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="e6938-115">Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="e6938-115">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="e6938-116">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6938-116">Create a new .NET Core project.</span></span>

   <span data-ttu-id="e6938-117">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="e6938-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e6938-118">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="e6938-118">On Windows, open a command prompt.</span></span> <span data-ttu-id="e6938-119">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6938-119">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="e6938-120">Spusťte aplikaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="e6938-120">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="e6938-121">Přejděte do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="e6938-121">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="e6938-122">Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="e6938-122">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="e6938-123">Je čas na serveru @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="e6938-123">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="e6938-124">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="e6938-124">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="e6938-125">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="e6938-125">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="e6938-126">Instalace .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="e6938-126">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="e6938-127">Vytvořte složku pro nový projekt .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6938-127">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="e6938-128">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="e6938-128">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="e6938-129">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="e6938-129">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="e6938-130">Pokud jste nainstalovali novější verze sady SDK na váš počítač, vytvořte *global.json* soubor a vyberte 1.0.4 SDK.</span><span class="sxs-lookup"><span data-stu-id="e6938-130">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="e6938-131">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e6938-131">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="e6938-132">Obnovení balíčků.</span><span class="sxs-lookup"><span data-stu-id="e6938-132">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="e6938-133">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e6938-133">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="e6938-134">[Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestavení aplikace nejprve v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="e6938-134">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="e6938-135">Přejděte do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e6938-135">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end