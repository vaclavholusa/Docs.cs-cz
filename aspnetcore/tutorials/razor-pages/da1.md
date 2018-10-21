---
title: Aktualizovat generované stránky v aplikaci ASP.NET Core
author: rick-anderson
description: Zjistěte, jak aktualizovat generované stránky v aplikaci ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f47a68840a6307b69bc92a7b157037d91dce5422
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477212"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="d0452-103">Aktualizovat generované stránky v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0452-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="d0452-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d0452-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d0452-105">Máme o dobrý začátek aplikace movie, ale v prezentaci není ideální.</span><span class="sxs-lookup"><span data-stu-id="d0452-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="d0452-106">Nechceme zobrazíte čas (12:00:00 dop. na následujícím obrázku) a **ReleaseDate** by měl být **datum vydání** (dvě slova).</span><span class="sxs-lookup"><span data-stu-id="d0452-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otevřít v prohlížeči Chrome ukazující data o filmech aplikace Movie](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="d0452-108">Aktualizace generovaného kódu</span><span class="sxs-lookup"><span data-stu-id="d0452-108">Update the generated code</span></span>

<span data-ttu-id="d0452-109">Otevřít *Models/Movie.cs* a přidejte zvýrazněné řádky je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="d0452-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="d0452-110">Klikněte pravým tlačítkem myši klikněte na červenou vlnovkou čáru > **rychlé akce a Refaktoringy**.</span><span class="sxs-lookup"><span data-stu-id="d0452-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Zobrazí místní nabídku \*\* > rychlé akce a Refaktoringy \*\*.](da1/qa.png)

<span data-ttu-id="d0452-112">Vyberte `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="d0452-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  <span data-ttu-id="d0452-114">Visual Studio přidá `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="d0452-114">Visual Studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="d0452-115">[Předchozí: Práce s SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Další: doplnili o funkce hledání](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="d0452-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
