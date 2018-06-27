---
title: Přidejte do aplikace ASP.NET MVC základní vyhledávání
author: rick-anderson
description: Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: aca0835340977605cc84fad1970ac30fa1a9872a
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961539"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

Poznámka:: SQLlite je velká a malá písmena, takže budete muset hledat "Neodstraněných" a není "neodstraněných".

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

Změna `<form>` značky v *Views\movie\Index.cshtml* zobrazení syntaxe Razor k určení `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Předchozí - metody Kontroleru a zobrazení](controller-methods-views.md)
> [další – přidání polí](new-field.md)
