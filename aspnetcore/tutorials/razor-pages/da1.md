---
title: "Aktualizovat generovaného stránky"
author: rick-anderson
description: "Aktualizujte generovaného stránky s lepší zobrazení."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: ca6a0ac10b2675fb8e7f27bdb9d740777a5e5f4e
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="5535c-103">Aktualizovat generovaného stránky</span><span class="sxs-lookup"><span data-stu-id="5535c-103">Update the generated pages</span></span>

<span data-ttu-id="5535c-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5535c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5535c-105">Máme správné spuštění na filmová aplikace, ale není ideální prezentaci.</span><span class="sxs-lookup"><span data-stu-id="5535c-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="5535c-106">Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).</span><span class="sxs-lookup"><span data-stu-id="5535c-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="5535c-108">Aktualizace generovaný kód</span><span class="sxs-lookup"><span data-stu-id="5535c-108">Update the generated code</span></span>

<span data-ttu-id="5535c-109">Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:</span><span class="sxs-lookup"><span data-stu-id="5535c-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

<span data-ttu-id="5535c-110">Klikněte pravým tlačítkem na červenou vlnovkou řádku > ** rychlé akce a refaktoring **.</span><span class="sxs-lookup"><span data-stu-id="5535c-110">Right click on a red squiggly line > ** Quick Actions and Refactorings**.</span></span>

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](da1/qa.png)

<span data-ttu-id="5535c-112">Vyberte `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="5535c-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  <span data-ttu-id="5535c-114">Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="5535c-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
<span data-ttu-id="5535c-115">[Předchozí: Práce s SQL serveru LocalDB](xref:tutorials/razor-pages/sql)
[přidání vyhledávání](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="5535c-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Adding Search](xref:tutorials/razor-pages/search)</span></span>
