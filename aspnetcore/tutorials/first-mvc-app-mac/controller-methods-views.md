---
title: "Metody kontroleru a zobrazení v aplikaci ASP.NET MVC jádra"
author: rick-anderson
description: "Práce s metody kontroleru, zobrazení a DataAnnotations"
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-babe-babe-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 846a78a09cdde0cfdbcf35bb899c1822ebe8fea2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="ebc24-104">Metody kontroleru a zobrazení v aplikaci ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="ebc24-104">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="ebc24-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ebc24-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ebc24-106">Máme správné spuštění na filmová aplikace, ale prezentaci není ideální.</span><span class="sxs-lookup"><span data-stu-id="ebc24-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="ebc24-107">Neradi najdete v části času (12:00:00 AM na následujícím obrázku) a **ReleaseDate** by měla být dva slova.</span><span class="sxs-lookup"><span data-stu-id="ebc24-107">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Index zobrazení: Datum vydání je jedno slovo (bez mezery) a každý datum vydání film zobrazuje čas 12: 00](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="ebc24-109">Otevřete *Models/Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="ebc24-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="ebc24-110">Sestavte a spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ebc24-110">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="ebc24-111">[Předchozí - práce s SQLite](working-with-sql.md)
[další – přidání vyhledávání](search.md)</span><span class="sxs-lookup"><span data-stu-id="ebc24-111">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
