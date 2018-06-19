---
title: Metody kontroleru a zobrazení v aplikaci ASP.NET MVC jádra
author: rick-anderson
description: Práce s metody kontroleru, zobrazení a DataAnnotations
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 7a6a965d99742e7e06e6da82999dc60264cac6c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897875"
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="110cf-103">Metody kontroleru a zobrazení v aplikaci ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="110cf-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="110cf-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="110cf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="110cf-105">Máme správné spuštění na filmová aplikace, ale není ideální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="110cf-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="110cf-106">Neradi najdete v části času (12:00:00 AM na následujícím obrázku) a **ReleaseDate** by měla být dva slova.</span><span class="sxs-lookup"><span data-stu-id="110cf-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Index zobrazení: Datum vydání je jedno slovo (bez mezery) a každý datum vydání film zobrazuje čas 12: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="110cf-108">Otevřete *Models/Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="110cf-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="110cf-109">Sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="110cf-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="110cf-110">[Předchozí - práce s SQLite](working-with-sql.md)
> [další – přidání vyhledávání](search.md)</span><span class="sxs-lookup"><span data-stu-id="110cf-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
