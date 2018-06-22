---
title: Metody kontroleru a zobrazení v ASP.NET Core
author: rick-anderson
description: Naučte se pracovat s metody kontroleru, zobrazení a DataAnnotations v ASP.NET Core.
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 7cf42807eaba356cd090a08bba9357c3ec237087
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278437"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="b73c4-103">Metody kontroleru a zobrazení v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b73c4-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="b73c4-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b73c4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b73c4-105">Máme správné spuštění na filmová aplikace, ale není ideální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="b73c4-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="b73c4-106">Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být dva slova.</span><span class="sxs-lookup"><span data-stu-id="b73c4-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Index zobrazení: Datum vydání je jedno slovo (bez mezery) a každý datum vydání film zobrazuje čas 12: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="b73c4-108">Otevřete *Models/Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="b73c4-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="b73c4-109">Sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b73c4-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="b73c4-110">[Předchozí - práce s SQLite](working-with-sql.md)
> [další – přidání vyhledávání](search.md)</span><span class="sxs-lookup"><span data-stu-id="b73c4-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
