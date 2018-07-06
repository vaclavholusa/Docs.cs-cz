---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Relace prvků v OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Dokumentace Microsoftu
author: MikeWasson
description: 'Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy jste autoři; produkty mají dodavatelů. Použití protokolu OData, klienti se můžete dostat přes...'
ms.author: aspnetcontent
ms.date: 06/26/2014
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 98f65b068d8f22e3eeef48ca7fa441434939db8b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827943"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Relace prvků v OData v4 pomocí rozhraní ASP.NET Web API 2.2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy jste autoři; produkty mají dodavatelů. Použití protokolu OData, klienti můžete přejít přes relací prvků. Zadaný produkt, můžete najít dodavatele. Můžete také vytvořit nebo odebrat relace. Například můžete nastavit od dodavatele, produktu.
> 
> Tento kurz ukazuje, jak podporují tyto operace v OData v4, pomocí rozhraní ASP.NET Web API. Tento kurz vychází z kurzu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2.1
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Kurz verze
> 
> OData verze 3, naleznete v tématu [podpora relací prvků v OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Přidání Entity dodavatele

> [!NOTE]
> Tento kurz vychází z kurzu [vytvořit OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md).


Nejdřív potřebujeme související entity. Přidejte třídu pojmenovanou `Supplier` ve složce modely.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Přidání navigační vlastnost, která má `Product` třídy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Přidat nový **DbSet** k `ProductsContext` třídy, aby rozhraní Entity Framework obsahovala dodavatele tabulky v databázi.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

V WebApiConfig.cs, přidejte &quot;Dodavatelé&quot; sada entit k modelu entity data model:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Přidání Kontroleru dodavatelů

Přidat `SuppliersController` třídy do složky řadiče.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Můžu nebude ukazují, jak přidat operace CRUD pro tento kontroler. Postup je stejný jako u kontroleru produktů (viz [vytvoření koncového bodu OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Získat související entity

Pokud chcete získat od dodavatele pro produkt, klient odešle požadavek GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Chcete-li tuto žádost o podporu, přidejte následující metodu do `ProductsController` třídy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Tato metoda používá výchozí zásady vytváření názvů

- Název metody: metody GetX, kde X je navigační vlastnost.
- Název parametru: *klíč*

Pokud budete postupovat podle této zásady vytváření názvů, webové rozhraní API automaticky mapují požadavku HTTP k metodě kontroleru.

Příklad HTTP žádosti:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Příklad HTTP odpovědi:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Získání související kolekce

V předchozím příkladu má produkt jednoho dodavatele. Navigační vlastnost také může vrátit kolekci. Následující kód načte produktů pro dodavatele:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

V takovém případě vrátí metoda **IQueryable** místo **SingleResult&lt;T&gt;**

Příklad HTTP žádosti:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Příklad HTTP odpovědi:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Vytvoření relace mezi entitami

OData podporuje vytváření nebo odebírání relací mezi dva existující entity. V terminologii OData v4, je vztah &quot;odkaz&quot;. (V OData v3 relace byla volána *odkaz*. Rozdíly protokolu na mezerách nezáleží pro účely tohoto kurzu.)

Odkaz má svůj vlastní identifikátor URI, že je formulář `/Entity/NavigationProperty/$ref`. Například tady je identifikátor URI odkazu mezi produktu a jeho dodavatele řešení:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Přidání relace, klient odešle požadavek POST a PUT na tuto adresu.

- Pokud se vlastnost navigace, jako je jedna entita, `Product.Supplier`.
- PŘÍSPĚVEK, pokud navigační vlastnost, jako je kolekce, `Supplier.Products`.

Text žádosti obsahuje identifikátor URI z druhé entity ve vztahu. Tady je příklad žádosti:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

V tomto příkladu, klient odešle požadavek PUT pro `/Products(6)/Supplier/$ref`, což je identifikátor URI $ref `Supplier` produkt s ID = 6. Pokud neproběhne úspěšně, odešle server odezvu 204 (žádný obsah):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Tady je metoda kontroleru Přidat relaci `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty* parametr určuje, které vztah k nastavení. (Pokud existuje více než jednu vlastnost navigace u entity, můžete přidat další `case` příkazy.)

*Odkaz* parametr obsahuje identifikátor URI od dodavatele. Webové rozhraní API automaticky analyzuje text požadavku má být získána hodnota tohoto parametru.

K vyhledání od dodavatele, potřebujeme ID (nebo klíče), který je součástí *odkaz* parametru. Chcete-li to provést, použijte následující pomocnou metodu:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

V podstatě tato metoda používá knihovnou OData rozdělit do segmentů cesty identifikátoru URI, najděte segment, který obsahuje klíč a převést na správný typ klíče.

## <a name="deleting-a-relationship-between-entities"></a>Odstraňuje se relace mezi entitami

Odstranění relace, klient odešle požadavek HTTP DELETE na identifikátor URI $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Tady je metoda kontroleru odstranit vztah mezi produktem a Dodavatel:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

V takovém případě `Product.Supplier` je &quot;1&quot; konec vztah 1: n, takže vztahu můžete odebrat tak, že nastavení `Product.Supplier` k `null`.

V &quot;mnoho&quot; konec relace, klient musí určovat, která související entitu, kterou chcete odebrat. Uděláte to tak, klient odešle v řetězci dotazu požadavku URI související entity. Například chcete-li odebrat "Produktu 1" z "1" na dodavatele:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

V rozhraní Web API z toho důvodu musíme zahrnout další parametr v `DeleteRef` metody. Tady je metoda kontroleru odebrat produkt ze `Supplier.Products` vztah.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*Klíč* parametr je klíč pro dodavatele a *relatedKey* parametr je klíčem k produktu k odebrání `Products` vztah. Všimněte si, že webové rozhraní API automaticky získá klíč z řetězce dotazu.
