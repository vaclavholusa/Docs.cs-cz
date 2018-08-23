---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Část 6: Vytvoření Kontrolerů produktů a objednávek | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752020"
---
<a name="part-6-creating-product-and-order-controllers"></a>Část 6: Vytvoření Kontrolerů produktů a objednávek
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Přidání Kontroleru produkty

Kontroleru pro správce je pro uživatele, kteří mají oprávnění správce. Zákazníci, na druhé straně může zobrazit produkty, ale nejde vytvořit, aktualizovat nebo je odstranit.

Jsme můžete snadno omezit přístup na metody Post, Put a Delete a ponechání otevřít metody Get. Ale podívejte se na data, která je vrácena pro produkt:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost` Vlastnosti by neměly být viditelné pro zákazníky! Toto řešení je k definování *objekt pro přenos dat* (DTO), který obsahuje podmnožinu vlastností, které by měly být viditelné pro zákazníky. Použijeme k projektu LINQ `Product` instance na `ProductDTO` instancí.

Přidejte třídu pojmenovanou `ProductDTO` ke složce modely.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Nyní přidejte kontroler. V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče. Vyberte **přidat**a pak vyberte **řadič**. V **přidat kontroler** dialogového okna, názvu kontroleru &quot;ProductsController&quot;. V části **šablony**vyberte **kontroler API s prázdnou**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Všechno, co ve zdrojovém souboru nahraďte následujícím kódem:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Kontroler stále používá `OrdersContext` dáte dotaz na databázi. Ale místo vrácení `Product` instance přímo, říkáme `MapProducts` do projektu je na `ProductDTO` instancí:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts` Vrátí metoda **IQueryable**, takže jsme tvoří výsledek s další parametry dotazu. Vidíte to `GetProduct` metodu, která přidá **kde** klauzule dotazu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Přidat kontroler Orders

V dalším kroku přidáte kontroler, který umožňuje uživatelům vytvořit a zobrazit příkazy.

Začneme jiný objekt DTO. V Průzkumníku řešení klikněte pravým tlačítkem na složku modely a přidejte třídu pojmenovanou `OrderDTO` použijte následující implementaci:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Nyní přidejte kontroler. V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče. Vyberte **přidat**a pak vyberte **řadič**. V **přidat kontroler** dialogové okno, nastavte následující možnosti:

- V části **názvu Kontroleru**, zadejte "OrdersController".
- V části **šablony**vyberte "Kontroler API s akcemi čtení/zápisu, používá nástroj Entity Framework".
- V části **třída modelu**vyberte &quot;pořadí (ProductStore.Models)&quot;.
- V části **třída kontextu dat**vyberte &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Klikněte na tlačítko **přidat**. Tím se přidá soubor s názvem OrdersController.cs. Dále musíme upravit výchozí implementace kontroleru.

Nejprve odstraňte `PutOrder` a `DeleteOrder` metody. V tomto příkladu zákazníkům nemůžete upravovat ani odstranit existující objednávky. V reálné aplikaci je třeba velké množství back-end logiku pro tyto případy zpracují. (Například byl pořadí již dodán?)

Změnit `GetOrders` metoda vrátí objednávky, které patří uživateli:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Změnit `GetOrder` metodu následujícím způsobem:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Tady jsou změny, které jsme provedli metodu:

- Vrácená hodnota je `OrderDTO` instance, místo `Order`.
- Když zadáme dotaz databázi pro objednávku, používáme [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metoda načíst související `OrderDetail` a `Product` entity.
- Můžeme sloučit výsledek pomocí projekce.

Odpověď HTTP bude obsahovat pole s množství produktů:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Tento formát je jednodušší klienti využívat než původní graf objektu, který obsahuje vnořené entity (pořadí, podrobnosti a produkty).

Poslední metody, která považuje za `PostOrder`. V tuto chvíli, tato metoda přebírá `Order` instance. Ale zvažte, co se stane, pokud klient odešle textu žádosti podobně jako toto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Toto je dobře strukturovaných pořadí a Entity Framework se využívá elastic vložit do databáze. Ale obsahuje entitu produktů, které dříve neexistoval. Klient právě vytvořili nového produktu v databázi! Bude jím neočekávaném pořadí jsou oddělení, když se jim objednávku medvídek nese. Je ponaučení, buďte velmi opatrní při data, která můžete přijmout v požadavku POST a PUT.

Chcete-li tomuto problému vyhnout, změňte `PostOrder` metoda provést `OrderDTO` instance. Použití `OrderDTO` vytvořit `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Všimněte si, že používáme `ProductID` a `Quantity` vlastnosti a My ignorovat všechny hodnoty, které klient zaslal pro název produktu nebo cenu. Pokud ID produktu není platná, ji budou porušovat omezení cizího klíče v databázi a insert se nezdaří, jak by mělo.

Tady je úplný `PostOrder` metody:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Nakonec přidejte **Authorize** atribut kontroleru:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Jenom registrovaní uživatelé nyní můžete vytvořit nebo zobrazení objednávek.

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-5.md)
> [další](using-web-api-with-entity-framework-part-7.md)
