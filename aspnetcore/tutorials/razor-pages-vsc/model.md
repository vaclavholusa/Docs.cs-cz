---
title: "Přidání modelu do aplikace pro stránky Razor pomocí sady Visual Studio pro Mac"
author: rick-anderson
description: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core pomocí sady Visual Studio pro Mac"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 2e20e660bfa4aa335b951e2dc770e3671f99d2a8
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-code"></a><span data-ttu-id="a1ba5-103">Přidání modelu do aplikace stránky Razor ve ASP.NET Core s kódem jazyka Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a1ba5-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio Code</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a1ba5-104">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="a1ba5-104">Add a data model</span></span>

* <span data-ttu-id="a1ba5-105">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="a1ba5-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="a1ba5-106">Přidání třídy k *modely* složku s názvem *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="a1ba5-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="a1ba5-107">Přidejte následující kód, který *Models/Movie.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="a1ba5-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="a1ba5-108">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="a1ba5-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="a1ba5-109">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="a1ba5-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="a1ba5-110">Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="a1ba5-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="a1ba5-111">K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="a1ba5-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="a1ba5-112">**Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="a1ba5-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="a1ba5-113">Upravit *RazorPagesMovie.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="a1ba5-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="a1ba5-114">Vyberte **soubor** > **otevření souboru**a pak vyberte *RazorPagesMovie.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="a1ba5-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="a1ba5-115">Přidat odkaz na nástroj pro `Microsoft.EntityFrameworkCore.Tools.DotNet` druhým  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="a1ba5-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="a1ba5-116">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="a1ba5-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="a1ba5-117">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="a1ba5-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a1ba5-118">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a1ba5-118">Run the following command:</span></span>

<span data-ttu-id="a1ba5-119">**Poznámka: Spusťte následující příkaz v systému Windows. Systému MacOS a Linux najdete v části další příkaz**</span><span class="sxs-lookup"><span data-stu-id="a1ba5-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="a1ba5-120">V systému MacOS a Linux spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a1ba5-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="a1ba5-121">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="a1ba5-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="a1ba5-122">Visual Studio ukončete a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="a1ba5-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]
<span data-ttu-id="a1ba5-123">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a1ba5-123">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a1ba5-124">[Předchozí: Začínáme](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="a1ba5-124">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
