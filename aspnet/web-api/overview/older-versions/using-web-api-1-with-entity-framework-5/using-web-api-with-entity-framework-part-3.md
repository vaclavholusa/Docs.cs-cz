---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Část 3: Vytvoření Kontroleru pro správce | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: b9d21edec7b5006beea83395cdfc5ae181992e7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365042"
---
<a name="part-3-creating-an-admin-controller"></a>Část 3: Vytvoření Kontroleru pro správce
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Přidání Kontroleru pro správce

V této části přidáme kontroler Web API, která podporuje CRUD (vytváření, čtení, aktualizace a odstranění) týkající se produktů. Kontroler pomocí Entity Framework komunikovat s databázové vrstvě. Pouze Administrátoři budou moci použít tento kontroler. Zákazníci budou přistupovat prostřednictvím jiného řadiče produktů.

V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče. Vyberte **přidat** a potom **řadič**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

V **přidat kontroler** dialogového okna, názvu kontroleru `AdminController`. V části **šablony**vyberte &quot;kontroler API s akcemi čtení/zápisu, používá nástroj Entity Framework&quot;. V části **třída modelu**, vyberte "Produktu (ProductStore.Models)". V části **kontext dat**vyberte "&lt;nový kontext dat&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Pokud **třída modelu** rozevíracího seznamu nezobrazuje žádné třídy modelu, ujistěte se, že kompilaci projektu. Entity Framework používá reflexi, proto musí zkompilovaného sestavení.


Výběr "&lt;nový kontext dat.&gt;" se otevře **nový kontext dat** dialogového okna. Název kontextu dat `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Klikněte na tlačítko **OK** zrušíte **nový kontext dat** dialogového okna. V **přidat kontroler** dialogového okna, klikněte na tlačítko **přidat**.

Zde je, co byl přidán do projektu:

- Třída s názvem `OrdersContext` , která je odvozena z **DbContext**. Tato třída poskytuje skutečným pojidlem mezi modely POCO a databází.
- Kontroler Web API s názvem `AdminController`. Tento kontroler podporuje operace CRUD v `Product` instancí. Používá `OrdersContext` třídy ke komunikaci s Entity Framework.
- Nový připojovací řetězec databáze v souboru Web.config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Otevřete soubor OrdersContext.cs. Všimněte si, že konstruktor Určuje název připojovacího řetězce databáze. Tento název se odkazuje na připojovací řetězec, který byl přidán do souboru Web.config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Přidejte následující vlastnosti pro `OrdersContext` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

A **DbSet** představuje sadu entit, které je možné zadávat dotazy. Tady je úplný seznam pro `OrdersContext` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

`AdminController` Třída definuje pět metod, které implementují základních funkcí CRUD. Každá metoda odpovídá identifikátoru URI, který může vyvolat klienta:

| Metoda kontroleru | Popis | Identifikátor URI | Metoda HTTP |
| --- | --- | --- | --- |
| GetProducts | Získá všechny produkty. | rozhraní API a produktů | ZÍSKAT |
| GetProduct | Vyhledá produktů podle ID. | rozhraníAPI/produkty/*id* | ZÍSKAT |
| PutProduct | Aktualizace produktu. | rozhraníAPI/produkty/*id* | PUT |
| PostProduct | Vytvoří nový produkt. | rozhraní API a produktů | PŘÍSPĚVEK |
| DeleteProduct | Odstraní produktu. | rozhraníAPI/produkty/*id* | DELETE |

Každá metoda volá `OrdersContext` dáte dotaz na databázi. Volání metody, které upravit kolekci (PUT, POST a DELETE) `db.SaveChanges` a zachová tak změny do databáze. Řadiče jsou vytvořené na požadavek HTTP a poté odstraněn, proto je potřeba předtím, než vrátí metodu zachování změn.

## <a name="add-a-database-initializer"></a>Přidat inicializační výraz databáze

Entity Framework obsahuje skvělé funkce, která vám umožní naplnit databázi při spuštění a při každé změně modelů automaticky znovu vytvořit databázi. Tato funkce je užitečná při vývoji, protože budete mít vždy nějaká testovací data, i když změníte modely.

V Průzkumníku řešení klikněte pravým tlačítkem na složku modely a vytvořte novou třídu s názvem `OrdersContextInitializer`. Vložte následující implementaci:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Dědění z **DropCreateDatabaseIfModelChanges** třídy, jsme o tom, Entity Framework pro odpojení databáze pokaždé, když upravíme tříd modelu. Pokud rozhraní Entity Framework vytvoří (nebo znovu vytvoří) databáze, volání **počáteční hodnoty** metoda naplnění tabulek. Používáme **počáteční hodnoty** způsob, jak přidat některé produkty příklad plus příklad objednávky.

Tato funkce se skvěle hodí pro testování, ale nepoužívají **DropCreateDatabaseIfModelChanges** třídy v produkčním prostředí, protože pokud někdo přechází na jiné třídy modelu může přijít o data.

Dále otevřete Global.asax a přidejte následující kód, který **aplikace\_Start** metody:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Odeslat požadavek na Kontroleru

V tuto chvíli jsme nenapsali jakýkoli kód klienta, ale můžete vyvolat webové rozhraní API pomocí webového prohlížeče nebo ladění HTTP nástroj, jako [Fiddler](http://www.fiddler2.com/fiddler2/). V sadě Visual Studio stisknutím klávesy F5 spusťte ladění. Ve webovém prohlížeči se otevře na `http://localhost:*portnum*/`, kde *portnum* je nějaké číslo portu.

Odeslání požadavku HTTP "`http://localhost:*portnum*/api/admin`. První požadavek může být pomalé dokončit, protože Entify Framework potřebuje k vytvoření a přidání dat do databáze. Odpověď by měla něco podobného následujícímu:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-2.md)
> [další](using-web-api-with-entity-framework-part-4.md)
