---
title: "Přidání vyhledávání"
author: rick-anderson
description: "Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra"
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-8ef6-4628-855d-200206d962b9
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: f811cd0063404872b0e993a99e8a92cc354064ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="49753-104">Můžete rychle přejmenovat `searchString` parametru `id` s **přejmenovat** příkaz.</span><span class="sxs-lookup"><span data-stu-id="49753-104">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="49753-105">Klikněte pravým tlačítkem na `searchString` **> přejmenujte**.</span><span class="sxs-lookup"><span data-stu-id="49753-105">Right click on `searchString` **> Rename**.</span></span>

![Kontextové nabídky](search/_static/rename.png)

<span data-ttu-id="49753-107">Přejmenování cíle jsou vyznačené.</span><span class="sxs-lookup"><span data-stu-id="49753-107">The rename targets are highlighted.</span></span>

![Editor kódu zobrazuje proměnnou zvýrazněných v celém indexu ActionResult metoda](search/_static/rename2.png)

<span data-ttu-id="49753-109">Změňte parametr pro `id` a všechny výskyty `searchString` změnit na `id`.</span><span class="sxs-lookup"><span data-stu-id="49753-109">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor kódu zobrazuje proměnná se změnil na id](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="49753-111">Všimněte si, jak intelliSense pomáhá nám aktualizovat kód.</span><span class="sxs-lookup"><span data-stu-id="49753-111">Notice how intelliSense helps us update the markup.</span></span>

![Kontextové nabídky IntelliSense s vybraného v seznamu atributů pro form element – metoda](search/_static/int_m.png)

![Kontextové nabídky IntelliSense s získat vybraného v seznamu hodnot atributu – metoda](search/_static/int_get.png)

<span data-ttu-id="49753-114">Všimněte si rozlišovací písma v `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="49753-114">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="49753-115">Rozlišovací písma označuje značky podporuje [značky Pomocníci](../../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="49753-115">That distinctive font indicates the tag is supported by [Tag Helpers](../../mvc/views/tag-helpers/intro.md).</span></span>

![Formulářová značka fialové textem.](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="49753-117">[Předchozí](controller-methods-views.md)
[další](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="49753-117">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
