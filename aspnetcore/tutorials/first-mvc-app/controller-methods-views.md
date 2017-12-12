---
title: "Metody kontroleru a zobrazení"
author: rick-anderson
description: "Práce s metody kontroleru, zobrazení a DataAnnotations"
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="fa386-104">Metody kontroleru a zobrazení</span><span class="sxs-lookup"><span data-stu-id="fa386-104">Controller methods and views</span></span>

<span data-ttu-id="fa386-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fa386-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fa386-106">Máme správné spuštění na filmová aplikace, ale prezentaci není ideální.</span><span class="sxs-lookup"><span data-stu-id="fa386-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="fa386-107">Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být dva slova.</span><span class="sxs-lookup"><span data-stu-id="fa386-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Index zobrazení: Datum vydání je jedno slovo (bez mezery) a každý datum vydání film zobrazuje čas 12: 00](working-with-sql/_static/m55.png)

<span data-ttu-id="fa386-109">Otevřete *Models/Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="fa386-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="fa386-110">Klikněte pravým tlačítkem na červenou vlnovkou řádku **> rychlé akce a refaktoring**.</span><span class="sxs-lookup"><span data-stu-id="fa386-110">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](controller-methods-views/_static/qa.png)


<span data-ttu-id="fa386-112">Klepněte na`using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="fa386-112">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](controller-methods-views/_static/da.png)

  <span data-ttu-id="fa386-114">Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="fa386-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="fa386-115">Umožňuje odebrat `using` příkazy, které nejsou potřebné.</span><span class="sxs-lookup"><span data-stu-id="fa386-115">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="fa386-116">Se zobrazí ve výchozím nastavení v světla šedé písma.</span><span class="sxs-lookup"><span data-stu-id="fa386-116">They show up by default in a light grey font.</span></span> <span data-ttu-id="fa386-117">Klikněte pravým tlačítkem na libovolné místo v *Movie.cs* soubor **> odebrat a řazení direktiv Using**.</span><span class="sxs-lookup"><span data-stu-id="fa386-117">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Odebrat a řazení direktiv Using](controller-methods-views/_static/rm.png)

<span data-ttu-id="fa386-119">Aktualizovaný kódu:</span><span class="sxs-lookup"><span data-stu-id="fa386-119">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="fa386-120">[Předchozí](working-with-sql.md)
[další](search.md)</span><span class="sxs-lookup"><span data-stu-id="fa386-120">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
