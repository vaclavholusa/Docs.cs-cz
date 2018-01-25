---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "Část 6: Vytvoření produktu a pořadí řadiče | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 3b33543f02479b97112a63eb3879967ae31ccfb3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="part-6-creating-product-and-order-controllers"></a>Část 6: Vytvoření produktu a pořadí řadiče
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Přidat řadič produkty

Správce řadiče je pro uživatele, kteří mají oprávnění správce. Zákazníci, na druhé straně, můžete zobrazit produkty, ale nelze vytvořit, aktualizovat nebo je odstranit.

Jsme můžete snadno omezit přístup pro metodu Post, Put a Delete a nechat otevřené metody Get. Ale podívejte se na data, která je vrácena produktu:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost` Vlastnosti by neměly být viditelné zákazníkům! Řešení je k definování *objekt pro přenos dat* (DTO), který obsahuje podmnožinu vlastností, které by měly být viditelné pro zákazníky. Použijeme k projektu LINQ `Product` instance k `ProductDTO` instance.

Přidejte třídu s názvem `ProductDTO` ke složce modelů.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Nyní přidejte kontroleru. V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče. Vyberte **přidat**, pak vyberte **řadič**. V **přidat kontroler** dialogové okno, názvu kontroleru &quot;ProductsController&quot;. V části **šablony**, vyberte **kontroler API prázdný**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Všechno, co ve zdrojovém souboru nahraďte následujícím kódem:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Řadičem stále používá `OrdersContext` dotazu databázi. Ale namísto vrácení `Product` instance přímo, říkáme `MapProducts` do projektu je do `ProductDTO` instancí:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts` Metoda vrátí **IQueryable**, takže jsme můžete vytvořit výsledek s jinými parametry. Zobrazí se v `GetProduct` metodu, která přidá **kde** klauzule dotazu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Přidat řadič objednávky

V dalším kroku přidáte kontroler, který umožňuje uživatelům vytvářet a prohlížet objednávky.

Začneme s jinou DTO. V Průzkumníku řešení klikněte pravým tlačítkem na složku modely a přidejte třídu s názvem `OrderDTO` použijte následující implementace:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Nyní přidejte kontroleru. V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče. Vyberte **přidat**, pak vyberte **řadič**. V **přidat kontroler** dialogové okno, nastavte následující možnosti:

- V části **názvu Kontroleru**, zadejte "OrdersController".
- V části **šablony**, vyberte možnost "Kontroler API s akcemi čtení/zápisu používající rozhraní Entity Framework".
- V části **třída modelu**, vyberte &quot;pořadí (ProductStore.Models)&quot;.
- V části **třída kontextu dat**, vyberte &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Klikněte na tlačítko **přidat**. Tento postup přidá soubor s názvem OrdersController.cs. Dále je potřeba upravit výchozí implementaci třídy kontroleru.

Nejprve odstraňte `PutOrder` a `DeleteOrder` metody. Tato ukázka zákazníci nelze upravit nebo odstranit existující objednávky. V reálné aplikaci bude třeba velké množství logiku back-end pro tyto případy zpracují. (Například byl pořadí již dodán?)

Změna `GetOrders` metoda vrátí objednávky, které patří uživateli:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Změna `GetOrder` metoda následujícím způsobem:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Zde jsou změny, které jsme provedli metody:

- Vrácená hodnota je `OrderDTO` instance, místo `Order`.
- Když jsme dotazování databáze pro pořadí, používáme [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metoda načíst související `OrderDetail` a `Product` entity.
- Výsledek jsme vyrovnání pomocí projekce.

Odpověď HTTP bude obsahovat pole produkty s počty:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Tento formát je jednodušší, aby klienti mohli využívat než původní grafu objektu, která obsahuje vnořené entity (pořadí, podrobnosti a produkty).

Poslední metoda ji zvažovat `PostOrder`. Teď, tato metoda přebírá `Order` instance. Ale zvažte, co se stane, pokud klient pošle obsah žádosti takto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Toto je dobře strukturovaných zakázky a Entity Framework se brouka vložit do databáze. Ale obsahuje entity produktu, který dříve neexistoval. Klient právě vytvořili nového produktu v naší databázi! Bude jím neočekávaném pořadí jsou oddělení, když se jim pořadí pro medvídek nese. Je ponaučení, skutečně opatrně data, která je přijmout v požadavku POST nebo PUT.

Chcete-li se tomuto problému vyhnout, změňte `PostOrder` metoda provést `OrderDTO` instance. Použití `OrderDTO` vytvořit `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Všimněte si, že používáme `ProductID` a `Quantity` vlastnosti a My ignorujte všechny hodnoty, které klient zaslal pro název produktu nebo cena. Pokud není platné ID produktu, nesplňují omezení cizího klíče v databázi a úlohy insert se nezdaří, jako by měl.

Tady je kompletní `PostOrder` metoda:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Nakonec přidejte **Autorizovat** atribut kontroleru:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Jenom registrovaní uživatelé nyní můžete vytvořit nebo zobrazit objednávky.

>[!div class="step-by-step"]
[Předchozí](using-web-api-with-entity-framework-part-5.md)
[další](using-web-api-with-entity-framework-part-7.md)
