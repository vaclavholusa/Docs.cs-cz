---
title: Metody kontroleru a zobrazení v aplikaci ASP.NET MVC jádra
author: rick-anderson
description: Práce s metody kontroleru, zobrazení a DataAnnotations
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 0bd57597c74918829de38764326e35df08ab1e05
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279074"
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a>Metody kontroleru a zobrazení v aplikaci ASP.NET MVC jádra

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme správné spuštění na filmová aplikace, ale není ideální prezentaci. Neradi najdete v části času (12:00:00 AM na následujícím obrázku) a **ReleaseDate** by měla být dva slova.

![Index zobrazení: Datum vydání je jedno slovo (bez mezery) a každý datum vydání film zobrazuje čas 12: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Otevřete *Models/Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Sestavte a spusťte aplikaci.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Předchozí - práce s SQLite](working-with-sql.md)
> [další – přidání vyhledávání](search.md)
