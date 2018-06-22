---
title: Přidání vyhledávání
author: rick-anderson
description: Ukazuje, jak přidat hledání do jednoduchou aplikaci ASP.NET MVC jádra
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: fb93d9688c9abf76ad0057c646c4b7662d003108
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274837"
---
[!INCLUDE [adding-model](~/includes/mvc-intro/search1.md)]

Můžete rychle přejmenovat `searchString` parametru `id` s **přejmenovat** příkaz. Klikněte pravým tlačítkem na `searchString` **> přejmenujte**.

![Kontextové nabídky](search/_static/rename.png)

Přejmenování cíle jsou vyznačené.

![Editor kódu zobrazuje proměnnou zvýrazněných v celém indexu ActionResult metoda](search/_static/rename2.png)

Změňte parametr pro `id` a všechny výskyty `searchString` změnit na `id`.

![Editor kódu zobrazuje proměnná se změnil na id](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

Všimněte si, jak intelliSense pomáhá nám aktualizovat kód.

![Kontextové nabídky IntelliSense s vybraného v seznamu atributů pro form element – metoda](search/_static/int_m.png)

![Kontextové nabídky IntelliSense s získat vybraného v seznamu hodnot atributu – metoda](search/_static/int_get.png)

Všimněte si rozlišovací písma v `<form>` značky. Rozlišovací písma označuje značky podporuje [značky Pomocníci](~/mvc/views/tag-helpers/intro.md).

![Formulářová značka fialové textem.](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Předchozí](controller-methods-views.md)
> [další](new-field.md)  
