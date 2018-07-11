---
title: Aktualizovat generované stránky v aplikaci ASP.NET Core
author: rick-anderson
description: Zjistěte, jak aktualizovat generované stránky v aplikaci ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 55ff98712da314e28e50a1b1b1e04530d5b3fedd
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186228"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualizovat generované stránky v aplikaci ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme o dobrý začátek aplikace movie, ale v prezentaci není ideální. Nechceme zobrazíte čas (12:00:00 dop. na následujícím obrázku) a **ReleaseDate** by měl být **datum vydání** (dvě slova).

![Otevřít v prohlížeči Chrome ukazující data o filmech aplikace Movie](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizace generovaného kódu

Otevřít *Models/Movie.cs* a přidejte zvýrazněné řádky je znázorněno v následujícím kódu:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]
::: moniker-end

Klikněte pravým tlačítkem myši klikněte na červenou vlnovkou čáru > **rychlé akce a Refaktoringy**.

  ![Zobrazí místní nabídku ** > rychlé akce a Refaktoringy **.](da1/qa.png)

Vyberte `using System.ComponentModel.DataAnnotations;`

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  Visual studio přidá `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Předchozí: Práce s SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [vyhledávání přidat](xref:tutorials/razor-pages/search)
