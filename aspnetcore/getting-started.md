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
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="b0f95-103">Začínáme s ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0f95-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="b0f95-104">Nainstalujte [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="b0f95-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="b0f95-105">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0f95-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="b0f95-106">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="b0f95-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="b0f95-107">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="b0f95-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="b0f95-108">Zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b0f95-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="b0f95-109">Spusťte aplikaci pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="b0f95-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="b0f95-110">Přejděte do [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="b0f95-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="b0f95-111">Otevřete *Pages/About.cshtml* a upravte stránku a zobrazí se zpráva "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="b0f95-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="b0f95-112">Je čas na serveru @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="b0f95-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="b0f95-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="b0f95-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="b0f95-114">Přejděte do [ http://localhost:5000/About ](http://localhost:5000/About) a ověřit změny.</span><span class="sxs-lookup"><span data-stu-id="b0f95-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="b0f95-115">Instalace .NET Core **instalační program sady SDK** pro sadu SDK 1.0.4 z [.NET Core všechny soubory ke stažení stránky](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="b0f95-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="b0f95-116">Vytvořte složku pro nový projekt .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0f95-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="b0f95-117">V systému macOS a Linux otevřete okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="b0f95-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="b0f95-118">V systému Windows otevřete příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="b0f95-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="b0f95-119">Pokud jste nainstalovali novější verze sady SDK na váš počítač, vytvořte *global.json* soubor a vyberte 1.0.4 SDK.</span><span class="sxs-lookup"><span data-stu-id="b0f95-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="b0f95-120">Vytvoření nového projektu .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b0f95-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="b0f95-121">Obnovení balíčků.</span><span class="sxs-lookup"><span data-stu-id="b0f95-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="b0f95-122">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b0f95-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="b0f95-123">[Dotnet spustit](/dotnet/core/tools/dotnet-run) příkaz sestavení aplikace nejprve v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="b0f95-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="b0f95-124">Přejděte do `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b0f95-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end