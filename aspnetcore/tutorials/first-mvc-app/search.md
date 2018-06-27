---
title: Přidejte do aplikace ASP.NET MVC základní vyhledávání
author: rick-anderson
description: Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: d0581142b505323385feba441b707d29b3f6db5d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960837"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

<span data-ttu-id="2ffaf-103">Můžete rychle přejmenovat `searchString` parametru `id` s **přejmenovat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="2ffaf-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="2ffaf-104">Klikněte pravým tlačítkem na `searchString` **> přejmenujte**.</span><span class="sxs-lookup"><span data-stu-id="2ffaf-104">Right click on `searchString` **> Rename**.</span></span>

![Kontextové nabídky](search/_static/rename.png)

<span data-ttu-id="2ffaf-106">Přejmenování cíle jsou vyznačené.</span><span class="sxs-lookup"><span data-stu-id="2ffaf-106">The rename targets are highlighted.</span></span>

![Editor kódu zobrazuje proměnnou zvýrazněných v celém indexu ActionResult metoda](search/_static/rename2.png)

<span data-ttu-id="2ffaf-108">Změňte parametr pro `id` a všechny výskyty `searchString` změnit na `id`.</span><span class="sxs-lookup"><span data-stu-id="2ffaf-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor kódu zobrazuje proměnná se změnil na id](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

<span data-ttu-id="2ffaf-110">Všimněte si, jak intelliSense pomáhá nám aktualizovat kód.</span><span class="sxs-lookup"><span data-stu-id="2ffaf-110">Notice how intelliSense helps us update the markup.</span></span>

![Kontextové nabídky IntelliSense s vybraného v seznamu atributů pro form element – metoda](search/_static/int_m.png)

![Kontextové nabídky IntelliSense s získat vybraného v seznamu hodnot atributu – metoda](search/_static/int_get.png)

<span data-ttu-id="2ffaf-113">Všimněte si rozlišovací písma v `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="2ffaf-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="2ffaf-114">Rozlišovací písma označuje značky podporuje [značky Pomocníci](~/mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="2ffaf-114">That distinctive font indicates the tag is supported by [Tag Helpers](~/mvc/views/tag-helpers/intro.md).</span></span>

![Formulářová značka fialové textem.](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="2ffaf-116">[Předchozí](controller-methods-views.md)
> [další](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="2ffaf-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
