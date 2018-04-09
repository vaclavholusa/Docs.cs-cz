---
title: Aktualizovat generovaného stránky v aplikaci ASP.NET Core
author: rick-anderson
description: Zjistěte, jak aktualizovat generovaného stránky v aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 0207696af14305a1b549cf55da1b42fa99d01776
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aktualizovat generovaného stránky v aplikaci ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme správné spuštění na filmová aplikace, ale není ideální prezentaci. Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizace generovaný kód

Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Klikněte pravým tlačítkem na červenou vlnovkou řádku > ** rychlé akce a refaktoring **.

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](da1/qa.png)

Vyberte `using System.ComponentModel.DataAnnotations;`

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Předchozí: Práce s SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [přidat vyhledávání](xref:tutorials/razor-pages/search)
