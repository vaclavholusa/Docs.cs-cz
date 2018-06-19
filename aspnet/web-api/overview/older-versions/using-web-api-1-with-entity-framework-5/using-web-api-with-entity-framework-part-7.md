---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Část 7: Vytvoření hlavní stránky | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/10/2018
ms.locfileid: "30869865"
---
<a name="part-7-creating-the-main-page"></a>Část 7: Vytvoření hlavní stránky
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Vytváření hlavní stránky

V této části vytvoříte stránky hlavní aplikace. Tato stránka bude složitější než stránky pro správu, proto jsme budete postupovat v několika krocích. Na této cestě se zobrazí některé pokročilejší Knockout.js techniky. Tady je základní rozložení stránky:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produkty" obsahuje řadu produktů.
- "Košíku" obsahuje řadu produktů s počty. Kliknutím na tlačítko "Přidat do košíku" aktualizujete košíku.
- "Objednávky" obsahuje pole pořadí ID.
- "Informace" obsahuje podrobnosti o pořadí, což je pole položky (produkty s počty)

Začneme definováním některé základní rozložení ve formátu HTML, bez vazby dat nebo skriptu. Otevřete soubor Views/Home/Index.cshtml a veškerý obsah, nahraďte následujícím textem:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

V dalším kroku přidat oddíl skripty a vytvořit prázdný model zobrazení:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Podle návrhu šrafují dříve, našeho modelu zobrazení musí pozorovatelné objekty pro produkty, košíku, objednávek a podrobnosti. Přidejte následující proměnné na `AppViewModel` objektu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Uživatele můžete přidat do košíku položky ze seznamu produktů a odebrání položek z košíku. K zapouzdření tyto funkce, vytvoříme jiné třídy modelu zobrazení, která představuje produkt. Přidejte následující kód, který `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel` Třída obsahuje dvě funkce, které se používají k přesun produktu do a z košíku: `addItemToCart` přidá do košíku, jednu jednotku produktu a `removeAllFromCart` odebere všechny počty produktu.

Uživatele můžete vybrat existující pořadí a získat podrobnosti o pořadí. Zapouzdříme tuto funkci do jiného modelu zobrazení:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` Inicializován s pořadí, a podrobnosti o pořadí ho načte odesláním požadavek AJAX na server.

Všimněte si také, `total` vlastnost `OrderDetailsViewModel`. Tato vlastnost je zvláštní druh názvem lze zobrazit [počítaný lze zobrazit](http://knockoutjs.com/documentation/computedObservables.html). Jak již název napovídá, počítaný lze zobrazit umožňuje vazbě dat na vypočtenou hodnotou&#8212;v tomto případě celkové náklady pořadí.

Dál přidejte tyto funkce `AppViewModel`:

- `resetCart` Odebere všechny položky z košíku.
- `getDetails` Získá podrobnosti pro pořadí (podle pusing a nové `OrderDetailsViewModel` na `details` seznamu).
- `createOrder` Vytvoří nové pořadí a vyprázdní košíku.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Nakonec inicializovat model zobrazení tak, že požadavky AJAX pro produkty a objednávky:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK velké množství kódu, který je ale jsme je sestavena si krok za krokem, takže zpravidla návrhu je zrušte. Nyní můžete přidáme některé Knockout.js vazby v kódu HTML.

**Produkty**

Zde jsou vazby u seznamu produktu:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

To iteruje nad pole produkty a zobrazuje název a ceny. Tlačítko "Přidat do pořadí" je viditelná jenom v případě, že uživatel je přihlášen.

Volání tlačítko "Přidat do pořadí" `addItemToCart` na `ProductViewModel` instance pro produkt. Tento příklad ukazuje dobrý funkce Knockout.js: když model zobrazení obsahuje jinými modely zobrazení, můžete použít vazby pro vnitřní modelu. V tomto příkladu vazby v rámci `foreach` jsou použitá pro každé z `ProductViewModel` instance. Tento přístup je mnohem čisticí než uvedení všechny funkce do jednoho zobrazení modelu.

**Cart**

Zde jsou vazby u košíku:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

To iteruje nad pole košíku a zobrazí název, ceny a množství. Všimněte si, že tlačítko "Vytvořit pořadí" a "Odebrat" odkaz je vázána na modelu zobrazení funkce.

**Objednávky**

Zde jsou vazby u seznamu objednávky:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

To iteruje nad objednávky a zobrazuje ID pořadí. Událost kliknutím na odkaz je vázána `getDetails` funkce.

**Podrobnosti o objednávce**

Zde jsou vazby podrobnosti o pořadí:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

To iteruje nad položky v pořadí a zobrazí produktu, ceny a množství. Okolního div je viditelná pouze v případě, že pole podrobností obsahuje jednu nebo více položek.

## <a name="conclusion"></a>Závěr

V tomto kurzu jste vytvořili aplikaci, která používá rozhraní Entity Framework pro komunikaci s databáze a webového rozhraní API ASP.NET zajistit veřejné rozhraní na datové vrstvě. Používáme k vykreslení stránky HTML a Knockout.js a jQuery zajistit dynamické interakce bez zavážky stránky ASP.NET MVC 4.

Další zdroje informací:

- [ASP.NET Data Access Content Map](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Středisko pro vývojáře Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-6.md)
