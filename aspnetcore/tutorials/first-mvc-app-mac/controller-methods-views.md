---
title: "Metody kontroleru a zobrazení v aplikaci ASP.NET MVC jádra"
author: rick-anderson
description: "Práce s metody kontroleru, zobrazení a DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 154ff861161c5b96f80fe1496e91b68fe17767fe
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="dc436-103">Metody kontroleru a zobrazení v aplikaci ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="dc436-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="dc436-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dc436-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dc436-105">Máme správné spuštění na filmová aplikace, ale není ideální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="dc436-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="dc436-106">Neradi najdete v části času (12:00:00 AM na následujícím obrázku) a **ReleaseDate** by měla být dva slova.</span><span class="sxs-lookup"><span data-stu-id="dc436-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Index zobrazení: Datum vydání je jedno slovo (bez mezery) a každý datum vydání film zobrazuje čas 12: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="dc436-108">Otevřete *Models/Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="dc436-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="dc436-109">Sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="dc436-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="dc436-110">[Předchozí - práce s SQLite](working-with-sql.md)
[další – přidání vyhledávání](search.md)</span><span class="sxs-lookup"><span data-stu-id="dc436-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
