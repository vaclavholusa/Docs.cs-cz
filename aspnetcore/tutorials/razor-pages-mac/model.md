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
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 53ac6a5a530cdf4e58908f108bcdd0baa66da934
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="40ba2-103">Přidání modelu do aplikace stránky Razor ve ASP.NET Core pomocí sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="40ba2-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="40ba2-104">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="40ba2-104">Add a data model</span></span>

* <span data-ttu-id="40ba2-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** projektu a potom vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="40ba2-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="40ba2-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="40ba2-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="40ba2-107">Klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat** > **nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="40ba2-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="40ba2-108">V **nový soubor** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="40ba2-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="40ba2-109">Vyberte **Obecné** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="40ba2-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="40ba2-110">Vyberte **prázdné třídy** v problémové center.</span><span class="sxs-lookup"><span data-stu-id="40ba2-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="40ba2-111">Název třídy **film** a vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="40ba2-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="40ba2-112">Klikněte pravým tlačítkem na červenou vlnovkou řádek, například `MovieContext` v řádku `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="40ba2-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="40ba2-113">Vyberte **Quick Fix > pomocí RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="40ba2-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="40ba2-114">Visual studio. přidá na pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="40ba2-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="40ba2-115">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="40ba2-115">Build the project to verify you don't have any errors.</span></span>

![Vytvoření stránky](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="40ba2-117">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="40ba2-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="40ba2-118">Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="40ba2-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="40ba2-119">Klikněte na [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) odkaz získat číslo verze pro.</span><span class="sxs-lookup"><span data-stu-id="40ba2-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="40ba2-120">K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="40ba2-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="40ba2-121">**Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="40ba2-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="40ba2-122">Chcete-li upravit *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="40ba2-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="40ba2-123">Vyberte **soubor** > **otevřete**a pak vyberte *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="40ba2-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="40ba2-124">Vyberte **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="40ba2-124">Select **Options**.</span></span>
* <span data-ttu-id="40ba2-125">Změna **Otevřít protokolem** k **editoru zdrojového kódu**.</span><span class="sxs-lookup"><span data-stu-id="40ba2-125">Change **Open with** to **Source Code Editor**.</span></span>

![Upravte soubor csproj](model/csproj.png)

<span data-ttu-id="40ba2-127">Přidat `Microsoft.EntityFrameworkCore.Tools.DotNet` nástroj odkaz na druhý  **\<ItemGroup >**.:</span><span class="sxs-lookup"><span data-stu-id="40ba2-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="40ba2-128">V době psaní byly správné číslo verze vidět v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="40ba2-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="40ba2-129">Do projektu přidejte tyto stránek nebo filmy soubory</span><span class="sxs-lookup"><span data-stu-id="40ba2-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="40ba2-130">V sadě Visual Studio, klikněte pravým tlačítkem myši *stránky* složky a vyberte **Přidat > Přidat existující složku**.</span><span class="sxs-lookup"><span data-stu-id="40ba2-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="40ba2-131">Vyberte *filmy* složky.</span><span class="sxs-lookup"><span data-stu-id="40ba2-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="40ba2-132">V *Chosse soubory pro zahrnutí do projektu* dialogovém okně, vyberte **zahrnují všechny**.</span><span class="sxs-lookup"><span data-stu-id="40ba2-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="40ba2-133">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="40ba2-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="40ba2-134">[Předchozí: Začínáme](xref:tutorials/razor-pages-mac/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="40ba2-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
