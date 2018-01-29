---
title: "Přidání modelu do aplikace pro stránky Razor pomocí sady Visual Studio pro Mac"
author: rick-anderson
description: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core pomocí sady Visual Studio pro Mac"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 16d77b321ea1c30c2bbb1a47cd73b4cd136cdfe7
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/26/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="54c7d-103">Přidání modelu do aplikace stránky Razor ve ASP.NET Core pomocí sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="54c7d-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="54c7d-104">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="54c7d-104">Add a data model</span></span>

* <span data-ttu-id="54c7d-105">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="54c7d-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="54c7d-106">Přidání třídy k *modely* složku s názvem *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="54c7d-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="54c7d-107">Přidejte následující kód, který *Models/Movie.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="54c7d-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="54c7d-108">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="54c7d-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="54c7d-109">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="54c7d-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="54c7d-110">Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="54c7d-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="54c7d-111">K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="54c7d-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="54c7d-112">**Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="54c7d-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="54c7d-113">Upravit *RazorPagesMovie.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="54c7d-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="54c7d-114">Vyberte **soubor** > **otevření souboru**a pak vyberte *RazorPagesMovie.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="54c7d-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="54c7d-115">Přidat odkaz na nástroj pro `Microsoft.EntityFrameworkCore.Tools.DotNet` druhým  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="54c7d-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="54c7d-116">Vygenerované uživatelské rozhraní film modelu</span><span class="sxs-lookup"><span data-stu-id="54c7d-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="54c7d-117">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="54c7d-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="54c7d-118">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54c7d-118">Run the following command:</span></span>

<span data-ttu-id="54c7d-119">**Poznámka: Spusťte následující příkaz v systému Windows. Systému MacOS a Linux najdete v části další příkaz**</span><span class="sxs-lookup"><span data-stu-id="54c7d-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="54c7d-120">V systému MacOS a Linux spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54c7d-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="54c7d-121">Pokud dojde k chybě:</span><span class="sxs-lookup"><span data-stu-id="54c7d-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="54c7d-122">Visual Studio ukončete a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="54c7d-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]<span data-ttu-id="54c7d-123">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="54c7d-123"> The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="54c7d-124">[Předchozí: Začínáme](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="54c7d-124">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
