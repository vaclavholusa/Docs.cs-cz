---
title: Přidání vyhledávání do aplikace ASP.NET Core MVC
author: rick-anderson
description: Ukazuje, jak přidat hledání jednoduchou aplikaci ASP.NET Core MVC
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216193"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="3c01a-103">Rychle můžete přejmenovat `searchString` parametr `id` s **přejmenovat** příkazu.</span><span class="sxs-lookup"><span data-stu-id="3c01a-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="3c01a-104">Klikněte pravým tlačítkem na `searchString` **> přejmenujte**.</span><span class="sxs-lookup"><span data-stu-id="3c01a-104">Right click on `searchString` **> Rename**.</span></span>

![Kontextové nabídky](search/_static/rename.png)

<span data-ttu-id="3c01a-106">Přejmenování cíle jsou zvýrazněné.</span><span class="sxs-lookup"><span data-stu-id="3c01a-106">The rename targets are highlighted.</span></span>

![Editor kódu znázorňující proměnnou zvýrazní v celém indexu ActionResult – metoda](search/_static/rename2.png)

<span data-ttu-id="3c01a-108">Změňte parametr `id` a všechny výskyty `searchString` změnit na `id`.</span><span class="sxs-lookup"><span data-stu-id="3c01a-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor kódu znázorňující proměnná byla změněna na id](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="3c01a-110">Všimněte si, jak technologie intelliSense pomáhá nám aktualizovat značku.</span><span class="sxs-lookup"><span data-stu-id="3c01a-110">Notice how intelliSense helps us update the markup.</span></span>

![Kontextové nabídky technologie IntelliSense s metodou vybrali v seznamu atributů pro form element](search/_static/int_m.png)

![Kontextové nabídky technologie IntelliSense s získat vybrali v seznamu hodnot atributu – metoda](search/_static/int_get.png)

<span data-ttu-id="3c01a-113">Všimněte si, že zvláštní písmo `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="3c01a-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="3c01a-114">Rozlišovací písma označuje značky podporuje [pomocných rutin značek](~/mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="3c01a-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![značka formuláře s fialové text](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="3c01a-116">[Předchozí](controller-methods-views.md)
> [další](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="3c01a-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
