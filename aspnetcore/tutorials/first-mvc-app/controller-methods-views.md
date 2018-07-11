---
title: Metody kontroleru a zobrazení v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak pracovat s metodami kontroleru, zobrazení a DataAnnotations v ASP.NET Core.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: e94cb877576a68540a565225b2b3d79f9be53327
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38194011"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="c2b7a-103">Metody kontroleru a zobrazení v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2b7a-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="c2b7a-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c2b7a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c2b7a-105">Máme o dobrý začátek aplikace movie, ale v prezentaci není ideální.</span><span class="sxs-lookup"><span data-stu-id="c2b7a-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="c2b7a-106">Nechceme zobrazíte čas (12:00:00 dop. na následujícím obrázku) a **ReleaseDate** by měla být dvě slova.</span><span class="sxs-lookup"><span data-stu-id="c2b7a-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Index zobrazení: Datum vydání je jedno slovo (bez mezer) a každý film datum vydání ukazuje čas 12: 00](working-with-sql/_static/m55.png)

<span data-ttu-id="c2b7a-108">Otevřít *Models/Movie.cs* a přidejte zvýrazněné řádky je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="c2b7a-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]
::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="c2b7a-109">[Předchozí](working-with-sql.md)
> [další](search.md)</span><span class="sxs-lookup"><span data-stu-id="c2b7a-109">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
