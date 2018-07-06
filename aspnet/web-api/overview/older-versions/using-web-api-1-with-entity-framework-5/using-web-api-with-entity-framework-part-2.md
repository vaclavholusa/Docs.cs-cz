---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Část 2: Vytvoření doménových modelů | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 605a3eb5d781db6b317369efe8698288d7b9f648
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802267"
---
<a name="part-2-creating-the-domain-models"></a>Část 2: Vytvoření doménových modelů
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Přidání modelů

Existují tři způsoby, jak přístup Entity Framework:

- Databáze: začnete s databází a Entity Framework generuje kód.
- První model: začínat visual modelu a Entity Framework generuje v databázi a kódu.
- Založeno na kódu: spustit s kódem a Entity Framework generuje v databázi.

Používáme přístup založeno na kódu, proto začneme tak, že definujete naše objektů domény jako POCOs (prostý staré CLR objekty). S přístupem založeno na kódu domény objekty nemusí nějaký zvláštní kód pro podporu vrstva databáze, jako je například transakce nebo trvalost. (Konkrétně, není nutné dědění z [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) třídy.) Datové poznámky můžete stále řídit způsob, jak vytvořit schéma databáze v Entity Framework.

Protože POCOs nemají žádné další vlastnosti, které popisují [databáze stavu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), můžete je snadno serializovat do formátu JSON nebo XML. Však neznamená, že je potřeba vždy zveřejnit své modely Entity Framework přímo na klienty, jak uvidíme dále v tomto kurzu.

Vytvoříme POCOs následující:

- Produkt
- Pořadí
- OrderDetail

K vytvoření každé třídě, klikněte pravým tlačítkem na složku modely v Průzkumníku řešení. V místní nabídce vyberte **přidat** a pak vyberte **třídy.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Přidat `Product` třídy následující implementaci:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Podle konvence, Entity Framework používá `Id` vlastnost jako primární klíč a mapuje na sloupec identity v tabulce databáze. Když vytvoříte nový `Product` instance, nebude nastavena hodnotu `Id`, protože databáze generuje hodnotu.

**ScaffoldColumn** atribut oznamuje ASP.NET MVC, přejděte `Id` vlastnosti při generování pomocí editoru formuláře. **Vyžaduje** atribut se používá k ověření modelu. Určuje, že `Name` vlastnost musí být neprázdný řetězec.

Přidat `Order` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Přidat `OrderDetail` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Vztahy cizího klíče

Pořadí obsahuje mnoho podrobnostmi o objednávce a podrobnosti pro každé pořadí odkazuje na jednoho produktu. K reprezentaci tyto vztahy `OrderDetail` třídy definuje vlastnosti s názvem `OrderId` a `ProductId`. Entity Framework odvodí, že tyto vlastnosti představují cizího klíče a přidá omezení cizího klíče k databázi.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order` a `OrderDetail` třídy zahrnují také "navigační" vlastnosti, které obsahují odkazy na související objekty. Zadaný objednávky, můžete přejít na produkty v pořadí podle vlastnosti navigace.

Zkompilujte projekt. Entity Framework používá reflexi ke zjišťování vlastností modely, takže vyžaduje zkompilovaného sestavení k vytvoření schématu databáze.

## <a name="configure-the-media-type-formatters"></a>Konfigurace formátovací moduly typu médií

A [formátovací modul typu média](../../formats-and-model-binding/media-formatters.md) je objekt, který serializuje data, když webové rozhraní API zapíše text odpovědi HTTP. Předdefinované formátování podporuje výstup JSON a XML. Ve výchozím nastavení obě tyto formátování serializace všechny objekty podle hodnoty.

Serializace podle hodnoty vytvoří problém, pokud obsahuje grafu objektů cyklické odkazy. Je to přesně tento případ s `Order` a `OrderDetail` třídy, protože každá obsahuje odkaz na druhý. Formátovací modul bude postupovat podle odkazů, zápis každého objektu podle hodnoty a přejděte zacyklení. Proto je třeba změnit výchozí chování.

V Průzkumníku řešení rozbalte aplikace\_spusťte složky a otevřete soubor s názvem WebApiConfig.cs. Přidejte následující kód, který `WebApiConfig` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Tento kód nastaví formátování JSON pro zachování odkazy na objekty a zcela odebere formátovací modul XML z kanálu. (Můžete nakonfigurovat formátovací modul XML zachovat odkazy na objekty, ale je o něco více práce a potřebujeme jen JSON pro tuto aplikaci. Další informace najdete v tématu [zpracování cyklické odkazy objektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-1.md)
> [další](using-web-api-with-entity-framework-part-3.md)
