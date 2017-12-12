---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: "Část 3: Vytvoření správce řadiče | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 6fadfb6e96ae287fc5f81516b7535e03853c7e6a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-3-creating-an-admin-controller"></a>Část 3: Vytvoření správce řadiče
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Přidání správce řadiče

V této části přidáme kontroleru webového rozhraní API, která podporuje CRUD (vytvářet, číst, aktualizovat a odstranit) operací na produkty. Řadičem použije ke komunikaci s vrstva databáze Entity Framework. Pouze Administrátoři budou moci používat tento řadič. Zákazníci budou přistupovat prostřednictvím jiného řadiče produkty.

V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče. Vyberte **přidat** a potom **řadič**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

V **přidat kontroler** dialogové okno, názvu kontroleru `AdminController`. V části **šablony**, vyberte &quot;kontroler API s akcemi čtení/zápisu používající rozhraní Entity Framework&quot;. V části **třída modelu**, vyberte "Produkt (ProductStore.Models)". V části **kontextu dat**, vyberte možnost "&lt;nový kontext dat&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Pokud **třída modelu** rozevíracího seznamu nejsou zobrazeny všechny třídy modelu, zajistěte, aby zkompilovat projektu. Rozhraní Entity Framework používá reflexe, proto je nutné kompilované sestavení.


Výběr "&lt;nový kontext dat&gt;" se otevře **nový kontext dat** dialogové okno. Název kontextu dat `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Klikněte na tlačítko **OK** pro zavření **nový kontext dat** dialogové okno. V **přidat kontroler** dialogové okno, klikněte na tlačítko **přidat**.

Zde je, co byly přidány do projektu:

- Třída s názvem `OrdersContext` která je odvozena od **DbContext**. Tato třída poskytuje pojidlem mezi modely objektů POCO a databází.
- Kontroler Web API s názvem `AdminController`. Tento řadič podporujícího operace CRUD v `Product` instance. Použije `OrdersContext` třídy ke komunikaci s Entity Framework.
- Nový připojovací řetězec databáze v souboru Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Otevřete soubor OrdersContext.cs. Všimněte si, že konstruktoru Určuje název připojovacího řetězce databáze. Tento název se odkazuje na připojovací řetězec, který byl přidán do souboru Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Přidejte následující vlastnosti pro `OrdersContext` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet** představuje sadu entit, které může být dotazován. Tady je úplný seznam pro `OrdersContext` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController` Třída definuje pět metody, které implementují základní funkce CRUD. Každá metoda odpovídá identifikátoru URI, který lze vyvolat klienta:

| Metoda kontroleru | Popis | Identifikátor URI | Metoda HTTP |
| --- | --- | --- | --- |
| GetProducts | Získá všechny produkty. | rozhraní API nebo produkty | GET |
| GetProduct | Vyhledá produkt podle ID. | rozhraní API neboprodukty/*id* | GET |
| PutProduct | Aktualizace produktu. | rozhraní API neboprodukty/*id* | PUT |
| PostProduct | Vytvoří nového produktu. | rozhraní API nebo produkty | POST |
| DeleteProduct | Odstraní produktu. | rozhraní API neboprodukty/*id* | DELETE |

Každá metoda volá do `OrdersContext` dotazu databázi. Volání metody, které upravit kolekci (PUT, POST a odstranit) `db.SaveChanges` k zachování změn do databáze. Řadiče jsou vytvořené za požadavku HTTP a poté odstraněny, takže je potřeba předtím, než vrátí metodu zachování změn.

## <a name="add-a-database-initializer"></a>Přidat inicializátoru databáze

Rozhraní Entity Framework má dobrý funkce, která vám umožní naplnit databázi na spuštění a při každé změně modely automaticky znovu vytvoří databázi. Tato funkce je užitečná při vývoji, protože máte vždy nějaká testovací data, i když změníte modely.

V Průzkumníku řešení klikněte pravým tlačítkem na složku modely a vytvořte novou třídu s názvem `OrdersContextInitializer`. Vložte následující implementace:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Dědění ze **DropCreateDatabaseIfModelChanges** třída, jsme informace o tom, rozhraní Entity Framework pro odpojení databáze vždy, když jsme upravit třídy modelu. Když Entity Framework vytvoří (nebo vytvoří) databázi, zavolá **počáteční hodnoty** metoda k naplnění tabulky. Používáme **počáteční hodnoty** metody přidat některé produkty, příklad plus Příklad pořadí.

Tato funkce je skvělá pro testování, ale nepoužívají **DropCreateDatabaseIfModelChanges** třídy v produkčním prostředí, protože jste mohlo dojít ke ztrátě dat pokud někdo změní třídu modelu.

Potom otevřete soubor Global.asax a přidejte následující kód, který **aplikace\_spustit** metoda:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Odeslat požadavek na Kontroleru

V tuto chvíli jsme nebyly zapsány žádné kód klienta, ale můžete vyvolat webové rozhraní API pomocí webového prohlížeče nebo ladění HTTP nástroje, jako [Fiddler](http://www.fiddler2.com/fiddler2/). Ve Visual Studiu stisknutím klávesy F5 spusťte ladění. Otevře se webový prohlížeč k `http://localhost:*portnum*/`, kde *portnum* je některé číslo portu.

Odeslat požadavek HTTP "`http://localhost:*portnum*/api/admin`. První požadavek může být pomalé, protože Entify Framework nutné vytvořit a naplnit databázi. Odpověď by měla něco podobné následujícímu:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

>[!div class="step-by-step"]
[Předchozí](using-web-api-with-entity-framework-part-2.md)
[další](using-web-api-with-entity-framework-part-4.md)
