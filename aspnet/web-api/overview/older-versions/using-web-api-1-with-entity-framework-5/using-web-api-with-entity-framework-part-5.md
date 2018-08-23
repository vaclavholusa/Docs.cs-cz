---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Část 5: Vytvoření dynamického uživatelského rozhraní s knihovnou Knockout.js | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: d019941700992e173a5812b11b558b6726dfd809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754999"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Část 5: Vytvoření dynamického uživatelského rozhraní s knihovnou Knockout.js
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Vytvoření dynamického uživatelského rozhraní s knihovnou Knockout.js

V této části používáme rozhraní Knockout.js Přidání funkčnosti do zobrazení správce.

[Rozhraní Knockout.js](http://knockoutjs.com/) je knihovna jazyka Javascript, který usnadňuje vytvoření vazby ovládacích prvků HTML k datům. Rozhraní Knockout.js používá vzor Model-View-ViewModel (MVVM).

- *Modelu* je reprezentace dat v obchodní domény (v našich případě produktů a objednávky) na straně serveru.
- *Zobrazení* je prezentační vrstvy (HTML).
- *Model zobrazení* je objekt jazyka Javascript, která obsahuje data modelu. Model zobrazení je abstrakce kód uživatelského rozhraní. Nemá žádné znalosti jazyka HTML představující. Místo toho představuje abstraktní funkce zobrazení, jako je například "seznam položek".

Zobrazení je vázán na data modelu zobrazení. Aktualizace zobrazení modelu se automaticky projeví v zobrazení. Model zobrazení také načte události z zobrazení, jako je například kliknutí na tlačítko a provádí operace v modelu, jako je například vytvoření objednávky.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Budeme nejprve definovat model zobrazení. Potom jsme vytvoří vazbu modelu zobrazení bude značka jazyka HTML.

Přidejte následující část Razor na Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

V této části můžete přidat kamkoli v souboru. Když je zobrazení vykresleno, v části se zobrazí v dolní části stránky HTML, klikněte pravým tlačítkem myši před uzavírací značku &lt;/body&gt; značky.

Všechny skriptu pro tuto stránku půjdou uvnitř značky script indikován komentář:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Nejprve definujte třídu modelu zobrazení:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** je zvláštní druh objektu v Knockout, volá se, *pozorovat*. Z [dokumentaci rozhraní Knockout.js](http://knockoutjs.com/documentation/observables.html): existuje zjištěný je "objekt jazyka JavaScript, který může upozornit předplatitele o změnách." Při změně obsahu pozorovat, zobrazení se automaticky aktualizuje tak, aby odpovídaly.

K naplnění `products` pole, zkontrolujte požadavek AJAX pro webové rozhraní API. Připomínáme, že jsme uložené základní identifikátor URI pro rozhraní API v kontejneru a zobrazení (viz [část 4](using-web-api-with-entity-framework-part-4.md) kurzu).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Pak přidejte funkce do modelu zobrazení vytvářet, aktualizovat a odstraňovat produktů. Tyto funkce odeslání volání AJAX pro webové rozhraní API a použijte k aktualizaci modelu zobrazení výsledků.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Nyní nejdůležitější součástí: při modelu DOM se fulled načteno, volání **ko.applyBindings** fungovat a předejte mu novou instanci třídy `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings** metoda aktivuje Knockout a sváže model zobrazení do zobrazení.

Když teď máme zobrazení modelu, můžeme vytvořit vazby. V rozhraní Knockout.js, můžete to provést přidáním `data-bind` atributy prvků HTML. Například pokud chcete svázat pole seznamu HTML, použijte `foreach` vazby:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach` Vazby Iteruje přes pole a vytvoří podřízené prvky pro každý objekt v poli. Vazby na podřízené prvky mohou odkazovat na vlastnosti pole objektů.

Přidejte následující vazby na seznam "aktualizace produktů":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>` Element vyskytuje v rámci oboru **foreach** vazby. Že prostředky budou Knockout vykreslení elementu jednou pro každý produkt v `products` pole. Všechny vazby v rámci `<li>` element odkazují na tuto instanci produktu. Například `$data.Name` odkazuje `Name` vlastnost na produktu.

Pokud chcete nastavit hodnoty textovými vstupy, použijte `value` vazby. Tlačítka jsou vázány na funkce na zobrazení modelu pomocí `click` vazby. Instance produktu je předán jako parametr pro každou funkci. Další informace najdete [dokumentaci rozhraní Knockout.js](http://knockoutjs.com/documentation/observables.html) má dobrý popis různých vazby.

Dále přidejte vazbu pro **odeslat** událostí do formuláře přidat produktu:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Volá tuto vazbu `create` funkce na model zobrazení, chcete-li vytvořit nový produkt.

Tady je kompletní kód pro zobrazení správce:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Spusťte aplikaci, přihlaste se pomocí účtu správce a klikněte na odkaz "Admin". By měl zobrazit seznam produktů a být schopni vytvářet, aktualizovat nebo odstranit produktů.

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-4.md)
> [další](using-web-api-with-entity-framework-part-6.md)
