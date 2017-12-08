---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: "Vytvoření zobrazení (UI) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a>Vytvoření zobrazení (UI)
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

V této části začnete definovat HTML pro aplikaci a přidat vazby dat mezi HTML a zobrazení modelu.

Otevřete soubor Views/Home/Index.cshtml. Celý obsah souboru nahraďte následujícím textem.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Většina `div` elementy jsou na [Bootstrap](http://getbootstrap.com/) styly. Důležité elementy, jsou ty, s `data-bind` atributy. Tento atribut odkazů HTML do modelu zobrazení.

Příklad:

[!code-html[Main](part-7/samples/sample2.html)]

V tomto příkladu &quot; `text` &quot; vazby příčiny `<p>` elementu, který chcete zobrazit hodnotu `error` vlastnost z modelu zobrazení. Odvolat, `error` byla deklarována jako `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Vždy, když je přiřazen novou hodnotu `error`, Knockout text v aktualizovat `<p>` elementu.

`foreach` Vazby informuje Knockout můžete procházet obsah `books` pole. Pro každou položku v poli, vytvoří novou Knockout &lt;li&gt; elementu. Vazby v kontextu `foreach` odkazovat na vlastnosti položky pole. Příklad:

[!code-html[Main](part-7/samples/sample4.html)]

Zde `text` vazby čte vlastnost Autor každé adresáře.

Pokud nyní aplikaci spustíte, by měl vypadat takto:

![](part-7/_static/image1.png)

Seznam seznamů, načte asynchronně, po načtení stránky. Právě teď &quot;podrobnosti&quot; odkazy nejsou funkční. Tato funkce přidáme v další části.

>[!div class="step-by-step"]
[Předchozí](part-6.md)
[další](part-8.md)
