---
title: Metody kontroleru a zobrazení v ASP.NET Core
author: rick-anderson
description: Naučte se pracovat s metody kontroleru, zobrazení a DataAnnotations v ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a>Metody kontroleru a zobrazení v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme správné spuštění na filmová aplikace, ale není ideální prezentaci. Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být dva slova.

![Index zobrazení: Datum vydání je jedno slovo (bez mezery) a každý datum vydání film zobrazuje čas 12: 00](working-with-sql/_static/m55.png)

Otevřete *Models/Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Klikněte pravým tlačítkem na červenou vlnovkou řádku **> rychlé akce a refaktoring**.

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](controller-methods-views/_static/qa.png)


Klepněte na `using System.ComponentModel.DataAnnotations;`

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](controller-methods-views/_static/da.png)

  Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.

Umožňuje odebrat `using` příkazy, které nejsou potřebné. Se zobrazí ve výchozím nastavení v světla šedé písma. Klikněte pravým tlačítkem na libovolné místo v *Movie.cs* soubor **> odebrat a řazení direktiv Using**.

![Odebrat a řazení direktiv Using](controller-methods-views/_static/rm.png)

Aktualizovaný kódu:

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Předchozí](working-with-sql.md)
> [další](search.md)  
