---
title: Přidání modelu pro aplikace ASP.NET Core Razor Pages s Visual Studio Code
author: rick-anderson
description: Zjistěte, jak přidat modelu do mobilní aplikace Razor Pages v ASP.NET Core s využitím Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244720"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="a171c-103">Přidání modelu pro aplikace ASP.NET Core Razor Pages s Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a171c-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a171c-104">Přidání datového modelu</span><span class="sxs-lookup"><span data-stu-id="a171c-104">Add a data model</span></span>

* <span data-ttu-id="a171c-105">Přidat složku s názvem *modely*.</span><span class="sxs-lookup"><span data-stu-id="a171c-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="a171c-106">Přidat třídu *modely* složku s názvem *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="a171c-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="a171c-107">Přidejte následující kód, který *Models/Movie.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="a171c-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="a171c-108">Entity Framework Core NuGet balíček pro SQLite</span><span class="sxs-lookup"><span data-stu-id="a171c-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="a171c-109">Z příkazového řádku spusťte následující příkaz rozhraní příkazového řádku .NET Core:</span><span class="sxs-lookup"><span data-stu-id="a171c-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="a171c-110">Zaregistrujte kontext databáze</span><span class="sxs-lookup"><span data-stu-id="a171c-110">Register the database context</span></span>

<span data-ttu-id="a171c-111">Zaregistrovat kontext databáze s [injektáž závislostí](xref:fundamentals/dependency-injection) kontejneru *Startup.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="a171c-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

<span data-ttu-id="a171c-112">Přidejte následující `using` příkazů v horní části *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a171c-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="a171c-113">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="a171c-113">Build the project to verify you don't have any errors.</span></span>

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="a171c-114">Vygenerované uživatelské rozhraní Video modelu</span><span class="sxs-lookup"><span data-stu-id="a171c-114">Scaffold the Movie model</span></span>

* <span data-ttu-id="a171c-115">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="a171c-115">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a171c-116">**Pro Windows**: spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a171c-116">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="a171c-117">**Pro macOS a Linux**: spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a171c-117">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="a171c-118">V dalším kurzu vysvětluje souborů vytvořených databázovým generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="a171c-118">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a171c-119">[Předchozí: Začínáme](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Další: generované uživatelské rozhraní pro stránky Razor](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="a171c-119">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
