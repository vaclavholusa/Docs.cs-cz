---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Podpora relací prvků v OData v3 s webovým rozhraním API 2 | Dokumentace Microsoftu
author: MikeWasson
description: 'Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy jste autoři; produkty mají dodavatelů. Použití protokolu OData, klienti se můžete dostat přes...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: 311e84a2beb3ec7661fd650b277f23458bcb0cb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377474"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Podpora relací prvků v OData v3 s webovým rozhraním API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy jste autoři; produkty mají dodavatelů. Použití protokolu OData, klienti můžete přejít přes relací prvků. Zadaný produkt, můžete najít dodavatele. Můžete také vytvořit nebo odebrat relace. Například můžete nastavit od dodavatele, produktu.
> 
> Tento kurz ukazuje, jak podporují tyto operace v rozhraní ASP.NET Web API. Tento kurz vychází z kurzu [vytváření koncového bodu OData v3 s webovým rozhraním API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2
> - OData verze 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Přidání Entity dodavatele

Nejdřív potřebujeme přidat nový typ entity pro naše datového kanálu OData. Přidáme `Supplier` třídy.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Tato třída používá řetězce pro klíč entity. V praxi, který může být méně běžné než pomocí klíče celé číslo. Ale je vhodné zobrazuje, jak OData zpracovává jiné typy klíčů kromě celých čísel.

V dalším kroku vytvoříme vztah tak, že přidáte `Supplier` vlastnost `Product` třídy:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Přidat nový **DbSet** k `ProductServiceContext` třídy, aby obsahovala Entity Frameworku `Supplier` tabulky v databázi.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

V WebApiConfig.cs přidejte do modelu EDM entity "Dodavatelé":

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Navigační vlastnosti

Pokud chcete získat od dodavatele pro produkt, klient odešle požadavek GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Tady "Poskytovatel" je vlastnost navigace `Product` typu. V takovém případě `Supplier` odkazuje na jednu položku, ale navigační vlastnost také může vrátit kolekci (vztah jednoho k několika nebo many-to-many).

Chcete-li tuto žádost o podporu, přidejte následující metodu do `ProductsController` třídy:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*Klíč* parametr je klíč produktu. Metoda v tomto případě vrátí související entita & #8212 `Supplier` instance. Název metody i název parametru jsou důležité. Obecně platí Pokud navigační vlastnost s názvem "X", musíte přidat metodu s názvem "Metody GetX". Metoda přijme parametr s názvem "*klíč*", shoduje s datovým typem nadřazené položky klíče.

Je také důležité zahrnout **[FromOdataUri]** atribut *klíč* parametru. Tento atribut oznamuje webového rozhraní API pro použití pravidel syntaxe OData při analýze klíč z identifikátoru URI požadavku.

## <a name="creating-and-deleting-links"></a>Vytváření a odstraňování propojení

OData podporuje vytváření nebo odebírání vztahy mezi dvěma entitami. V terminologii OData je vztah "propojení". Každý odkaz má identifikátor URI s formulářem *entity*/$links /*entity*. Odkaz z produktu dodavateli například vypadá takto:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Pokud chcete vytvořit nový odkaz, klient odešle požadavek POST na identifikátor URI odkazu. Tělo žádosti je identifikátor URI cílové entity. Například předpokládejme, že je poskytovatel s klíčem "CTSO". Pro vytvoření odkazu z "Produktu1" k "Supplier('CTSO')", klient odešle požadavek vypadat asi takto:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Pokud chcete odstranit odkaz, klient odešle požadavek DELETE na identifikátor URI odkazu.

**Vytváření odkazů**

Pokud chcete povolit klienta k vytvoření odkazů produktu dodavatele, přidejte následující kód, který `ProductsController` třídy:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Tato metoda přijímá tři parametry:

- *klíč*: klíč, který chcete nadřazená entita (produkt)
- *objekt navigationProperty*: název navigační vlastnosti. V tomto příkladu je platné pouze navigační vlastnost "Poskytovatel".
- *odkaz*: URI protokolu OData související entity. Tato hodnota je převzata z textu požadavku. Například může být identifikátor URI odkazu "`http://localhost/odata/Suppliers('CTSO')`, což znamená dodavatele s ID ="CTSO".

Metoda používá odkaz k vyhledání od dodavatele. Pokud je nalezen odpovídající dodavatele, metoda nastaví `Product.Supplier` vlastnost a uloží výsledek do databáze.

Identifikátor URI odkazu je analýza nejtěžší část. V podstatě budete muset simulovat výsledek odeslat požadavek GET na tento identifikátor URI. Následující Pomocná metoda ukazuje, jak to provést. Metoda vyvolá proces směrování rozhraní Web API a získá zpět **objekt ODataPath** instanci, která představuje cestu k analyzovanou klauzuli OData. Odkaz na identifikátor URI by měla být jedna z segmenty klíč entity. (Pokud ne, klient zaslal chybný identifikátor URI.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Odstranění propojení**

Pokud chcete odstranit odkaz, přidejte následující kód, který `ProductsController` třídy:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

V tomto příkladu je navigační vlastnost jednoho `Supplier` entity. Pokud je navigační vlastnost kolekce, identifikátor URI pro odstranění odkazu musí obsahovat klíč související entity. Příklad:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Tento požadavek zruší zákazníka 1 pořadí 1. V tomto případě metodu DeleteLink mít následující podpis:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey* parametr poskytuje klíč pro související entity. Ano v vaše `DeleteLink` metoda vyhledávací primární entity podle *klíč* parametr, Najít související entity podle *relatedKey* parametr a potom odeberte přidružení. V závislosti na datovém modelu, může být nutné implementovat obě verze `DeleteLink`. Správná verze podle identifikátoru URI požadavku bude volat webové rozhraní API.
