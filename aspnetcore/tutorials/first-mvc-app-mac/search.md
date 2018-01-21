---
title: "Přidání hledání do jádra ASP.NET MVC aplikace"
author: rick-anderson
description: "Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 76739cd3805d9e5ac8b4b0e672b8e7da6596492e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="3c45d-103">Poznámka:: SQLlite je velká a malá písmena, takže budete muset hledat "Neodstraněných" a není "neodstraněných".</span><span class="sxs-lookup"><span data-stu-id="3c45d-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="3c45d-104">Změna `<form>` značky v *Views\movie\Index.cshtml* zobrazení syntaxe Razor k určení `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="3c45d-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="3c45d-105">[Předchozí - metody Kontroleru a zobrazení](controller-methods-views.md)
[další – přidání polí](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="3c45d-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
