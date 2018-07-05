---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Vytvoření zobrazení (uživatelského rozhraní) | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: e9ebe60f88ecbf65a6f8d04de9a23d72a72fda83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364978"
---
<a name="create-the-view-ui"></a>Vytvoření zobrazení (uživatelského rozhraní)
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](https://github.com/MikeWasson/BookService)

V této části se začnou definovat kód HTML pro aplikace a přidání datové vazby mezi HTML a model zobrazení.

Otevřete soubor Views/Home/Index.cshtml. Celý obsah tohoto souboru nahraďte následujícím kódem.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Většina `div` prvky jsou pro [Bootstrap](http://getbootstrap.com/) práce se styly. Důležité prvky jsou ty, které se `data-bind` atributy. Tento atribut odkazuje na model zobrazení HTML.

Příklad:

[!code-html[Main](part-7/samples/sample2.html)]

V tomto příkladu &quot; `text` &quot; vazby způsobí, že `<p>` prvek, který chcete zobrazit hodnotu `error` vlastnost z modelu zobrazení. Vzpomeňte si, že `error` byl deklarován jako `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Vždy, když je nová hodnota přiřazená `error`, Knockout aktualizace textu `<p>` element.

`foreach` Vazby říká Knockout pro cyklický průchod obsah `books` pole. Pro každou položku v poli, vytvoří novou Knockout &lt;li&gt; elementu. Vazby v kontextu `foreach` odkazují na vlastnosti pro položku pole. Příklad:

[!code-html[Main](part-7/samples/sample4.html)]

Tady `text` vazby načte vlastnost Autor jednotlivé knihy.

Pokud nyní aplikaci spustíte, by měl vypadat takto:

![](part-7/_static/image1.png)

Seznam knihy načte asynchronně, po načtení stránky. Právě teď &quot;podrobnosti&quot; odkazy nejsou funkční. Tato funkce přidáme v další části.

> [!div class="step-by-step"]
> [Předchozí](part-6.md)
> [další](part-8.md)
