---
title: Přidání modelu pro aplikace ASP.NET Core Razor Pages s Visual Studio Code
author: rick-anderson
description: Zjistěte, jak přidat modelu do mobilní aplikace Razor Pages v ASP.NET Core s využitím Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 3552b541c43375aef43838800855ec63e7fed372
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894040"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="a8e1f-103">Přidání modelu pro aplikace ASP.NET Core Razor Pages s Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a8e1f-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a8e1f-104">Přidání datového modelu</span><span class="sxs-lookup"><span data-stu-id="a8e1f-104">Add a data model</span></span>

* <span data-ttu-id="a8e1f-105">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="a8e1f-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="a8e1f-106">Přidat třídu *modely* složku s názvem *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="a8e1f-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="a8e1f-107">Přidejte následující kód, který *Models/Movie.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="a8e1f-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

<span data-ttu-id="a8e1f-108">Přidejte následující `using` příkazů v horní části *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8e1f-108">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="a8e1f-109">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="a8e1f-109">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="a8e1f-110">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="a8e1f-110">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="a8e1f-111">EF nástroje pro rozhraní příkazového řádku (CLI) jsou k dispozici v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="a8e1f-111">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="a8e1f-112">K instalaci tohoto balíčku, přidejte ji tak `DotNetCliToolReference` kolekce v *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="a8e1f-112">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="a8e1f-113">**Poznámka:** je potřeba nainstalovat tento balíček úpravou *.csproj* soubor; nelze použít `install-package` příkaz nebo grafické uživatelské rozhraní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="a8e1f-113">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="a8e1f-114">Upravit *RazorPagesMovie.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="a8e1f-114">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="a8e1f-115">Vyberte **souboru** > **otevřít soubor**a pak vyberte *RazorPagesMovie.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="a8e1f-115">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="a8e1f-116">Přidat odkaz na nástroj pro `Microsoft.EntityFrameworkCore.Tools.DotNet` druhé  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="a8e1f-116">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="a8e1f-117">Vygenerované uživatelské rozhraní Video modelu</span><span class="sxs-lookup"><span data-stu-id="a8e1f-117">Scaffold the Movie model</span></span>

* <span data-ttu-id="a8e1f-118">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="a8e1f-118">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a8e1f-119">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8e1f-119">Run the following command:</span></span>

<span data-ttu-id="a8e1f-120">**Poznámka: Spusťte následující příkaz na Windows. MacOS a Linux najdete v části další příkaz**</span><span class="sxs-lookup"><span data-stu-id="a8e1f-120">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="a8e1f-121">V systémech MacOS a Linux spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a8e1f-121">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="a8e1f-122">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="a8e1f-122">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="a8e1f-123">Ukončení sady Visual Studio a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="a8e1f-123">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="a8e1f-124">V dalším kurzu vysvětluje souborů vytvořených databázovým generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a8e1f-124">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a8e1f-125">[Předchozí: Začínáme](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Další: generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="a8e1f-125">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
