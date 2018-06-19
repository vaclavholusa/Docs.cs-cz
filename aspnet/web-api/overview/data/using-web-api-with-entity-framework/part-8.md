---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Zobrazit podrobnosti o položkách | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868084"
---
<a name="display-item-details"></a>Podrobnosti o položkách zobrazení
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

V této části bude přidána možnost zobrazení podrobností o jednotlivých adresáře. V app.js přidejte následující kód do modelu zobrazení:

[!code-javascript[Main](part-8/samples/sample1.js)]

V Views/Home/Index.cshtml přidá prvek ke data-bind odkaz Podrobnosti:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Tato obslužná rutina kliknutí na pro váže &lt;&gt; elementu, který chcete `getBookDetail` funkce na modelu zobrazení.

Ve stejném souboru nahraďte následující výstřižku:

[!code-html[Main](part-8/samples/sample3.html)]

tímto kódem:

[!code-html[Main](part-8/samples/sample4.html)]

Tento kód vytvoří tabulku, která je vázané na data do vlastnosti `detail` pozorovatelné v zobrazení modelu.

"&lt;!--Ko –&gt; &quot; syntaxe umožňuje zahrnout vazbu Knockout mimo elementu DOM. V takovém případě `if` vazby způsobí, že v této části kódu, který se zobrazí pouze tehdy, když `details` hodnotu Null.

[!code-html[Main](part-8/samples/sample5.html)]

Teď Pokud spusťte aplikaci a klikněte na jednu z &quot;podrobností&quot; odkazy, aplikace se zobrazí podrobnosti adresáře.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Předchozí](part-7.md)
> [další](part-9.md)
