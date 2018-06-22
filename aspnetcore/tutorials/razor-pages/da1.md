---
title: Aktualizovat generovaného stránky v aplikaci ASP.NET Core
author: rick-anderson
description: Zjistěte, jak aktualizovat generovaného stránky v aplikaci ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 55ff98712da314e28e50a1b1b1e04530d5b3fedd
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278067"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualizovat generovaného stránky v aplikaci ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme správné spuštění na filmová aplikace, ale není ideální prezentaci. Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizace generovaný kód

Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]
::: moniker-end

Klikněte pravým tlačítkem na červenou vlnovkou řádku > **rychlé akce a refaktoring**.

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](da1/qa.png)

Vyberte `using System.ComponentModel.DataAnnotations;`

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Předchozí: Práce s SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [přidat vyhledávání](xref:tutorials/razor-pages/search)
