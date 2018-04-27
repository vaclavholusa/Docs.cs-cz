---
title: Aktualizovat generovaného stránky v aplikaci ASP.NET Core
author: rick-anderson
description: Zjistěte, jak aktualizovat generovaného stránky v aplikaci ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 5c188799b7a42bcd5e9d5eab8dfe8cdad8002fe5
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/18/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="b2e4a-103">Aktualizovat generovaného stránky v aplikaci ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2e4a-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="b2e4a-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b2e4a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b2e4a-105">Máme správné spuštění na filmová aplikace, ale není ideální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="b2e4a-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="b2e4a-106">Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).</span><span class="sxs-lookup"><span data-stu-id="b2e4a-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="b2e4a-108">Aktualizace generovaný kód</span><span class="sxs-lookup"><span data-stu-id="b2e4a-108">Update the generated code</span></span>

<span data-ttu-id="b2e4a-109">Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:</span><span class="sxs-lookup"><span data-stu-id="b2e4a-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="b2e4a-110">Klikněte pravým tlačítkem na červenou vlnovkou řádku > ** rychlé akce a refaktoring **.</span><span class="sxs-lookup"><span data-stu-id="b2e4a-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](da1/qa.png)

<span data-ttu-id="b2e4a-112">Vyberte `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="b2e4a-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  <span data-ttu-id="b2e4a-114">Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="b2e4a-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="b2e4a-115">[Předchozí: Práce s SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [přidat vyhledávání](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="b2e4a-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
