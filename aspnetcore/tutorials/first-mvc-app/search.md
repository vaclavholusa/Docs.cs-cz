---
title: "Přidání vyhledávání"
author: rick-anderson
description: "Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 3ab9086275ec4c3651383c4c845e40db55f67f4c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

Můžete rychle přejmenovat `searchString` parametru `id` s **přejmenovat** příkaz. Klikněte pravým tlačítkem na `searchString` **> přejmenujte**.

![Kontextové nabídky](search/_static/rename.png)

Přejmenování cíle jsou vyznačené.

![Editor kódu zobrazuje proměnnou zvýrazněných v celém indexu ActionResult metoda](search/_static/rename2.png)

Změňte parametr pro `id` a všechny výskyty `searchString` změnit na `id`.

![Editor kódu zobrazuje proměnná se změnil na id](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Všimněte si, jak intelliSense pomáhá nám aktualizovat kód.

![Kontextové nabídky IntelliSense s vybraného v seznamu atributů pro form element – metoda](search/_static/int_m.png)

![Kontextové nabídky IntelliSense s získat vybraného v seznamu hodnot atributu – metoda](search/_static/int_get.png)

Všimněte si rozlišovací písma v `<form>` značky. Rozlišovací písma označuje značky podporuje [značky Pomocníci](../../mvc/views/tag-helpers/intro.md).

![Formulářová značka fialové textem.](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Předchozí](controller-methods-views.md)
[další](new-field.md)  
