---
title: Aktualizovat generovaného stránky v aplikaci ASP.NET Core
author: rick-anderson
description: Zjistěte, jak aktualizovat generovaného stránky v aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 0207696af14305a1b549cf55da1b42fa99d01776
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="c4e77-103">Aktualizovat generovaného stránky v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4e77-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="c4e77-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4e77-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4e77-105">Máme správné spuštění na filmová aplikace, ale není ideální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="c4e77-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="c4e77-106">Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).</span><span class="sxs-lookup"><span data-stu-id="c4e77-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="c4e77-108">Aktualizace generovaný kód</span><span class="sxs-lookup"><span data-stu-id="c4e77-108">Update the generated code</span></span>

<span data-ttu-id="c4e77-109">Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:</span><span class="sxs-lookup"><span data-stu-id="c4e77-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="c4e77-110">Klikněte pravým tlačítkem na červenou vlnovkou řádku > ** rychlé akce a refaktoring **.</span><span class="sxs-lookup"><span data-stu-id="c4e77-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](da1/qa.png)

<span data-ttu-id="c4e77-112">Vyberte `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="c4e77-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  <span data-ttu-id="c4e77-114">Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="c4e77-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="c4e77-115">[Předchozí: Práce s SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [přidat vyhledávání](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="c4e77-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
