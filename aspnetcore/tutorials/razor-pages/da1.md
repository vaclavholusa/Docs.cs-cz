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
ms.openlocfilehash: a1bb1ab1e4fac9c634f4048947ac3f934af3d625
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="update-the-generated-pages"></a>Aktualizovat generovaného stránky

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Máme správné spuštění na filmová aplikace, ale není ideální prezentaci. Neradi najdete v části času (12:00:00 AM na obrázku níže) a **ReleaseDate** by měla být **datum vydání** (dvě slova).

![Otevřete v prohlížeči Chrome zobrazující data film filmová aplikace](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aktualizace generovaný kód

Otevřete *Models/Movie.cs* souboru a přidejte následující kód ukazuje zvýrazněné řádky:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Klikněte pravým tlačítkem na červenou vlnovkou řádku > ** rychlé akce a refaktoring **.

  ![Kontextové nabídky ukazuje ** > rychlé akce a refaktoring **.](da1/qa.png)

Vyberte`using System.ComponentModel.DataAnnotations;`

  ![pomocí System.ComponentModel.DataAnnotations v horní části seznamu](da1/da.png)

  Visual studio. přidá `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE[model1](../../includes/RP/da2.md)]

>[!div class="step-by-step"]
[Předchozí: Práce s SQL serveru LocalDB](xref:tutorials/razor-pages/sql)
[přidání vyhledávání](xref:tutorials/razor-pages/search)
