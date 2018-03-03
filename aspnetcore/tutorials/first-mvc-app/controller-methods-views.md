---
title: "Metody kontroleru a zobrazení"
author: rick-anderson
description: "Práce s metody kontroleru, zobrazení a DataAnnotations"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 69f7bfa0f1e203c860abce06f6f2daffcaf47a59
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="55672-103">Metody kontroleru a zobrazení</span><span class="sxs-lookup"><span data-stu-id="55672-103">Controller methods and views</span></span>

<span data-ttu-id="55672-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="55672-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="55672-105">Máme správné spuštění na filmová aplikace, ale není ideální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="55672-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="55672-106">Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být dva slova.</span><span class="sxs-lookup"><span data-stu-id="55672-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Index zobrazení: Datum vydání je jedno slovo (bez mezery) a každý datum vydání film zobrazuje čas 12: 00](working-with-sql/_static/m55.png)

<span data-ttu-id="55672-108">Otevřete *Models/Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="55672-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="55672-109">Klikněte pravým tlačítkem na červenou vlnovkou řádku **> rychlé akce a refaktoring**.</span><span class="sxs-lookup"><span data-stu-id="55672-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](controller-methods-views/_static/qa.png)


<span data-ttu-id="55672-111">Klepněte na `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="55672-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](controller-methods-views/_static/da.png)

  <span data-ttu-id="55672-113">Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="55672-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="55672-114">Umožňuje odebrat `using` příkazy, které nejsou potřebné.</span><span class="sxs-lookup"><span data-stu-id="55672-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="55672-115">Se zobrazí ve výchozím nastavení v světla šedé písma.</span><span class="sxs-lookup"><span data-stu-id="55672-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="55672-116">Klikněte pravým tlačítkem na libovolné místo v *Movie.cs* soubor **> odebrat a řazení direktiv Using**.</span><span class="sxs-lookup"><span data-stu-id="55672-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Odebrat a řazení direktiv Using](controller-methods-views/_static/rm.png)

<span data-ttu-id="55672-118">Aktualizovaný kódu:</span><span class="sxs-lookup"><span data-stu-id="55672-118">The updated code:</span></span>

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="55672-119">[Předchozí](working-with-sql.md)
[další](search.md)</span><span class="sxs-lookup"><span data-stu-id="55672-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
