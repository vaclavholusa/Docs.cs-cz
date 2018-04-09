---
title: Přidání vyhledávání
author: rick-anderson
description: Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 772409f11a43e1d130265d8bba3bad1da5a41b86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="4174d-103">Můžete rychle přejmenovat `searchString` parametru `id` s **přejmenovat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="4174d-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="4174d-104">Klikněte pravým tlačítkem na `searchString` **> přejmenujte**.</span><span class="sxs-lookup"><span data-stu-id="4174d-104">Right click on `searchString` **> Rename**.</span></span>

![Kontextové nabídky](search/_static/rename.png)

<span data-ttu-id="4174d-106">Přejmenování cíle jsou vyznačené.</span><span class="sxs-lookup"><span data-stu-id="4174d-106">The rename targets are highlighted.</span></span>

![Editor kódu zobrazuje proměnnou zvýrazněných v celém indexu ActionResult metoda](search/_static/rename2.png)

<span data-ttu-id="4174d-108">Změňte parametr pro `id` a všechny výskyty `searchString` změnit na `id`.</span><span class="sxs-lookup"><span data-stu-id="4174d-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor kódu zobrazuje proměnná se změnil na id](search/_static/rename3.png)

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="4174d-110">Všimněte si, jak intelliSense pomáhá nám aktualizovat kód.</span><span class="sxs-lookup"><span data-stu-id="4174d-110">Notice how intelliSense helps us update the markup.</span></span>

![Kontextové nabídky IntelliSense s vybraného v seznamu atributů pro form element – metoda](search/_static/int_m.png)

![Kontextové nabídky IntelliSense s získat vybraného v seznamu hodnot atributu – metoda](search/_static/int_get.png)

<span data-ttu-id="4174d-113">Všimněte si rozlišovací písma v `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="4174d-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="4174d-114">Rozlišovací písma označuje značky podporuje [značky Pomocníci](../../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="4174d-114">That distinctive font indicates the tag is supported by [Tag Helpers](../../mvc/views/tag-helpers/intro.md).</span></span>

![Formulářová značka fialové textem.](search/_static/th_font.png)

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="4174d-116">[Předchozí](controller-methods-views.md)
> [další](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="4174d-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
