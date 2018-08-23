---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Část 7: Vytvoření hlavní stránky | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb4704e7f4f13fab04acdbdd642174884517e18a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755226"
---
<a name="part-7-creating-the-main-page"></a>Část 7: Vytvoření hlavní stránky
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Vytvoření hlavní stránky

V této části vytvoříte stránky hlavní aplikace. Tato stránka bude složitější než na stránce správy, takže jsme budete postupovat v několika krocích. Na cestě zobrazí se vám některé pokročilé techniky knihovnou Knockout.js. Tady je základní rozložení stránky:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produkty" obsahuje celou řadu produktů.
- "Košíku" obsahuje celou řadu produktů s množství. Kliknutím na tlačítko "Přidat do košíku" aktualizujete košíku.
- "Orders" obsahuje pole ID objednávek.
- "Details" obsahuje detaily objednávky, což je pole položky (produkty s množství)

Začneme tak, že definujete některé základní rozložení ve formátu HTML, bez vytváření datových vazeb nebo skriptu. Otevřete soubor Views/Home/Index.cshtml a veškerý obsah nahraďte následujícím kódem:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

V dalším kroku přidejte oddíl skripty a vytvořte prázdný model zobrazení:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Podle návrhu šrafují dříve, náš model zobrazení musí pozorovatelné objekty pro produkty, košík, objednávek a podrobnosti. Přidejte následující proměnné tak, `AppViewModel` objektu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Uživatele můžete přidat položky ze seznamu produktů do košíku a odebrání položek z košíku. K zapouzdření tyto funkce, vytvoříme jiné třídy zobrazení modelu, který představuje produkt. Přidejte následující kód, který `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel` Třída obsahuje dvě funkce, které se používají k přesunu produktu do a z košíku: `addItemToCart` přidá jednu jednotku produktu do nákupního košíku, a `removeAllFromCart` odebere všechny množství produktu.

Uživatele můžete vybrat existující pořadí a získat podrobnosti objednávky. Zapouzdříme tuto funkci do jiného zobrazení modelu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` Je inicializován s objednávkou, a načte podrobnosti objednávky odesláním požadavku AJAX na server.

Všimněte si také, `total` vlastnost `OrderDetailsViewModel`. Tato vlastnost je zvláštní druh pozorovat volána [vypočítané pozorovat](http://knockoutjs.com/documentation/computedObservables.html). Jak již název napovídá, počítaný pozorovat vám umožní vytvořit datovou vazbu s vypočtenou hodnotou&#8212;v tomto případě celkové náklady na pořadí.

V dalším kroku přidejte tyto funkce k `AppViewModel`:

- `resetCart` Odebere všechny položky z košíku.
- `getDetails` načte podrobnosti objednávky (podle pusing nový `OrderDetailsViewModel` na `details` seznamu).
- `createOrder` Vytvoří novou objednávku a vyprázdní košíku.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Nakonec inicializujte model zobrazení tím, že odesílání požadavků AJAX pro produktů a objednávek:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

Tak dobře, to je spousta kódu, ale sestavili jsme ji si krok za krokem, takže snad návrhu je jasné. Teď můžeme přidat některé knihovnou Knockout.js vazby v kódu HTML.

**Produkty**

Tady jsou vazby pro seznam produktů:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

To Iteruje přes pole produkty a zobrazí název a ceny. Tlačítko "Přidat do pořadí" je viditelná pouze v případě, že uživatel je přihlášen.

Volání tlačítko "Přidat do pořadí" `addItemToCart` na `ProductViewModel` instance pro produkt. Tento příklad ukazuje užitečnou funkci z rozhraní Knockout.js: při zobrazení modelu obsahuje jiné Zobrazit modely, můžete použít vazby vnitřní modelu. V tomto příkladu vazby v rámci `foreach` se použijí u všech `ProductViewModel` instancí. Tento přístup je mnohem přehlednější, než všechny funkce vložení do jednoho modelu zobrazení.

**Košíku**

Tady jsou vazby pro košíku:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

To Iteruje přes pole košíku a zobrazí název, cena a množství. Všimněte si, že odkaz "Odebrat" a "Vytvoření objednávky" tlačítko jsou vázány na model zobrazení funkce.

**Objednávky**

Tady jsou vazby u seznamu objednávek:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

To Iteruje přes objednávky a zobrazuje ID objednávky. Událost kliknutím na odkaz je vázána `getDetails` funkce.

**Podrobnosti objednávky**

Tady jsou vazby pro podrobnosti objednávky:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

To iteruje položky v pořadí a zobrazí produktu, cena a množství. Okolní div je viditelná pouze v případě, že pole Podrobnosti obsahuje jednu nebo více položek.

## <a name="conclusion"></a>Závěr

V tomto kurzu jste vytvořili aplikaci, která používá Entity Framework pro komunikaci s databází a ASP.NET Web API a poskytuje veřejná rozhraní na datové vrstvě. ASP.NET MVC 4 používáme k vykreslení stránky HTML a knihovnou Knockout.js plus jQuery zajistit dynamické interakce bez opětovné načtení stránky.

Další zdroje informací:

- [Mapa obsahu přístupu k datům v technologii ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Středisko pro vývojáře Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-6.md)
