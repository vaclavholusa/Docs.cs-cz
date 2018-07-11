---
title: Přidání modelu pro aplikace ASP.NET Core Razor Pages pomocí sady Visual Studio pro Mac
author: rick-anderson
description: Zjistěte, jak přidat modelu do mobilní aplikace Razor Pages v ASP.NET Core pomocí sady Visual Studio pro Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186755"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="fe500-103">Přidání modelu pro aplikace ASP.NET Core Razor Pages pomocí sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="fe500-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="fe500-104">Přidání datového modelu</span><span class="sxs-lookup"><span data-stu-id="fe500-104">Add a data model</span></span>

* <span data-ttu-id="fe500-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** projektu a pak vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="fe500-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="fe500-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="fe500-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="fe500-107">Klikněte pravým tlačítkem myši *modely* složku a pak vyberte **přidat** > **nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="fe500-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="fe500-108">V **nový soubor** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="fe500-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fe500-109">Vyberte **Obecné** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="fe500-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fe500-110">Vyberte **prázdnou třídu** v center usnadnit práci.</span><span class="sxs-lookup"><span data-stu-id="fe500-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="fe500-111">Název třídy **film** a vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="fe500-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="fe500-112">Klikněte pravým tlačítkem myši klikněte na červenou vlnovkou čáru, například `MovieContext` v řádku `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="fe500-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="fe500-113">Vyberte **Quick Fix > pomocí RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="fe500-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="fe500-114">Visual studio přidá nástroje pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="fe500-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="fe500-115">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="fe500-115">Build the project to verify you don't have any errors.</span></span>

![Vytvoření stránky](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="fe500-117">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="fe500-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="fe500-118">EF nástroje pro rozhraní příkazového řádku (CLI) jsou k dispozici v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="fe500-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="fe500-119">Klikněte na [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) odkaz na číslo verze používat.</span><span class="sxs-lookup"><span data-stu-id="fe500-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="fe500-120">K instalaci tohoto balíčku, přidejte ji tak `DotNetCliToolReference` kolekce v *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="fe500-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="fe500-121">**Poznámka:** je potřeba nainstalovat tento balíček úpravou *.csproj* soubor; nelze použít `install-package` příkaz nebo grafické uživatelské rozhraní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="fe500-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="fe500-122">Chcete-li upravit *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="fe500-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="fe500-123">Vyberte **souboru** > **otevřít**a pak vyberte *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="fe500-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="fe500-124">Vyberte **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="fe500-124">Select **Options**.</span></span>
* <span data-ttu-id="fe500-125">Změna **otevřít v programu** k **Editor zdrojového kódu**.</span><span class="sxs-lookup"><span data-stu-id="fe500-125">Change **Open with** to **Source Code Editor**.</span></span>

![Upravte soubor csproj](model/csproj.png)

<span data-ttu-id="fe500-127">Přidat `Microsoft.EntityFrameworkCore.Tools.DotNet` nástroj odkaz na druhý  **\<ItemGroup >**.:</span><span class="sxs-lookup"><span data-stu-id="fe500-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="fe500-128">V době psaní byly správné číslo verze je znázorněno v následujícím kódu.</span><span class="sxs-lookup"><span data-stu-id="fe500-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="fe500-129">Přidání stránky/filmy souborů do projektu</span><span class="sxs-lookup"><span data-stu-id="fe500-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="fe500-130">V sadě Visual Studio, klikněte pravým tlačítkem myši *stránky* a pak zvolte položku **Přidat > Přidat existující složku**.</span><span class="sxs-lookup"><span data-stu-id="fe500-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="fe500-131">Vyberte *filmy* složky.</span><span class="sxs-lookup"><span data-stu-id="fe500-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="fe500-132">V *vybrat soubory k zahrnutí do projektu* dialogového okna, vyberte **zahrnout všechny**.</span><span class="sxs-lookup"><span data-stu-id="fe500-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="fe500-133">V dalším kurzu vysvětluje souborů vytvořených databázovým generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fe500-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fe500-134">[Předchozí: Začínáme](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Další: generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="fe500-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
