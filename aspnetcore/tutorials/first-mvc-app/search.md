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

Rychle můžete přejmenovat `searchString` parametr `id` s **přejmenovat** příkazu. Klikněte pravým tlačítkem na `searchString` **> přejmenujte**.

![Kontextové nabídky](search/_static/rename.png)

Přejmenování cíle jsou zvýrazněné.

![Editor kódu znázorňující proměnnou zvýrazní v celém indexu ActionResult – metoda](search/_static/rename2.png)

Změňte parametr `id` a všechny výskyty `searchString` změnit na `id`.

![Editor kódu znázorňující proměnná byla změněna na id](search/_static/rename3.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search2.md)]

Všimněte si, jak technologie intelliSense pomáhá nám aktualizovat značku.

![Kontextové nabídky technologie IntelliSense s metodou vybrali v seznamu atributů pro form element](search/_static/int_m.png)

![Kontextové nabídky technologie IntelliSense s získat vybrali v seznamu hodnot atributu – metoda](search/_static/int_get.png)

Všimněte si, že zvláštní písmo `<form>` značky. Rozlišovací písma označuje značky podporuje [pomocných rutin značek](~/mvc/views/tag-helpers/intro.md).

![značka formuláře s fialové text](search/_static/th_font.png)

[!INCLUDE [adding-model](~/includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Předchozí](controller-methods-views.md)
> [další](new-field.md)  
