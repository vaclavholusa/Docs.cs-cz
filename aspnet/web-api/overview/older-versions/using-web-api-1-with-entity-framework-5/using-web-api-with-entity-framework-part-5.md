---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Část 5: Vytvoření dynamické uživatelského rozhraní s Knockout.js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Část 5: Vytvoření dynamické uživatelského rozhraní s Knockout.js
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Vytvoření s Knockout.js dynamické uživatelského rozhraní

V této části použijeme Knockout.js k přidání funkcí do zobrazení správce.

[Knockout.js](http://knockoutjs.com/) je knihovna jazyka Javascript, který usnadňuje vytvoření vazby ovládacích prvků HTML k datům. Knockout.js využívá vzor Model-View-ViewModel (modelem MVVM).

- *Modelu* je serverové reprezentace dat v doméně obchodní (v našem případu, produkty a objednávky).
- *Zobrazení* je prezentační vrstvy (HTML).
- *Modelu zobrazení* se objekt jazyka Javascript, který obsahuje data modelu. Model zobrazení je abstrakce kód uživatelského rozhraní. Nemá žádné znalosti reprezentace HTML. Místo toho představuje abstraktní funkce zobrazení, jako je například "seznam položek".

Zobrazení je vázané na data do modelu zobrazení. Aktualizace do modelu zobrazení, se automaticky promítnou v zobrazení. Model zobrazení také získá události ze zobrazení, jako je například kliknutí na tlačítko a provádí operace v modelu, například vytváření pořadí.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Nejprve budeme definovat model zobrazení. Potom jsme vytvoří vazbu kód HTML zobrazení modelu.

Přidejte následující části Razor Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

V této části můžete přidat libovolné místo v souboru. Když je zobrazení vykresleno, v dolní části stránky HTML, zobrazí se v části pravým před uzavírací &lt;/body&gt; značky.

Všechny skriptu pro tuto stránku přejde uvnitř značky script indikován komentář:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Nejprve definujte třídu modelu zobrazení:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** je zvláštní druh objektu v Knockout, volána *lze zobrazit*. Z [Knockout.js dokumentace](http://knockoutjs.com/documentation/observables.html): existuje zjištěný je "JavaScript objekt, který může upozornit odběratele o změnách." Při změně obsahu existuje zjištěný, se automaticky aktualizuje zobrazení tak, aby odpovídaly.

K naplnění `products` pole, je požadavek AJAX webovému rozhraní API. Odvolat jsme uložené základní identifikátor URI pro rozhraní API v kontejner zobrazení (viz [část 4](using-web-api-with-entity-framework-part-4.md) kurzu).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Dál přidejte funkce do modelu zobrazení vytvářet, aktualizovat a odstraňovat produkty. Tyto funkce odeslání volání AJAX webovému rozhraní API a aktualizovat model zobrazení pomocí výsledky.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Nyní nejdůležitější část: Pokud modelu DOM je fulled načíst, volání **ko.applyBindings** funkce a předávat novou instanci třídy `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

**Ko.applyBindings** metoda aktivuje Knockout a sváže model zobrazení do zobrazení.

Teď, když máme modelu zobrazení, můžeme vytvořit vazby. V Knockout.js, to uděláte tak, že přidáte `data-bind` atributy prvků HTML. Například do pole vytvořit vazbu seznamu HTML, použijte `foreach` vazby:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

`foreach` Vazba iteruje v rámci pole a vytvoří podřízené elementy pro každý objekt v poli. Vazby na podřízené elementy mohou odkazovat na vlastnosti na pole objektů.

Přidejte následující vazby do seznamu "aktualizace produkty":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

`<li>` Element dochází v rámci oboru **foreach** vazby. Aby znamená Knockout bude vykreslení elementu jednou pro každý produkt v `products` pole. Všechny vazby v rámci `<li>` element odkazovat na tuto instanci produktu. Například `$data.Name` odkazuje `Name` vlastnost na produktu.

Pokud chcete nastavit hodnoty vstupy text, použijte `value` vazby. Tlačítka je vázána na funkce na zobrazení modelu pomocí `click` vazby. Instance produktu je jako parametr předaný jednotlivé funkce. Další informace najdete [Knockout.js dokumentace](http://knockoutjs.com/documentation/observables.html) má dobrou popisy různých vazby.

V dalším kroku přidat vazbu pro **odeslání** událostí na formuláři přidat produkt:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Volá tuto vazbu `create` funkce na zobrazení modelu k vytvoření nového produktu.

Tady je kompletní kód pro zobrazení správce:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Spusťte aplikaci, přihlaste se pomocí účtu správce a klikněte na odkaz "Admin". By měl zobrazit seznam produktů a moct vytvářet, aktualizovat nebo odstranit produkty.

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-4.md)
> [další](using-web-api-with-entity-framework-part-6.md)
