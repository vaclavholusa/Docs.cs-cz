---
title: "Přidání vyhledávání"
author: rick-anderson
description: "Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra"
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-ffff-4628-855d-200206d96269
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 57419c697aea80a054e906c75002f5a39c512fc6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Poznámka:: SQLlite je velká a malá písmena, takže budete muset hledat "Neodstraněných" a není "neodstraněných".

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Změna `<form>` značky v *Views\movie\Index.cshtml* zobrazení syntaxe Razor k určení `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Předchozí - metody Kontroleru a zobrazení](controller-methods-views.md)
[další – přidání polí](new-field.md)  
