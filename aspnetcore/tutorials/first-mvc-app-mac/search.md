---
title: Přidání hledání do jádra ASP.NET MVC aplikace
author: rick-anderson
description: Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 4175d4dfd03d173f7025aff3b51d255bb1c213ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274479"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="416db-103">Poznámka:: SQLlite je velká a malá písmena, takže budete muset hledat "Neodstraněných" a není "neodstraněných".</span><span class="sxs-lookup"><span data-stu-id="416db-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="416db-104">Změna `<form>` značky v *Views\movie\Index.cshtml* zobrazení syntaxe Razor k určení `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="416db-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="416db-105">[Předchozí - metody Kontroleru a zobrazení](controller-methods-views.md)
> [další – přidání polí](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="416db-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
