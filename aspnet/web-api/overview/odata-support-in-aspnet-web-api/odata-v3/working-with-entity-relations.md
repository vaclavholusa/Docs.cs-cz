---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "Podpora vztahy entit v prostředí s Web API 2 OData v3 | Microsoft Docs"
author: MikeWasson
description: "Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy mít autoři; produkty mít dodavatelů. Použití protokolu OData, klienti můžete přejít přes..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Podpora vztahy entit v prostředí s Web API 2 OData v3
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy mít autoři; produkty mít dodavatelů. Použití protokolu OData, klienti můžete přejít přes vztahy entit. Zadaný produkt, můžete najít dodavatele. Můžete také vytvářet nebo odebírat vztahy. Například můžete nastavit dodavatele pro produkt.
> 
> Tento kurz ukazuje, jak pro podporu těchto operací v rozhraní ASP.NET Web API. Tento kurz je založený na kurz [vytváření koncový bod OData v3 s webovém rozhraní API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2
> - OData verze 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Přidání Entity dodavatele

Nejprve musíme přidejte nový typ entity pro naše datového kanálu OData. Přidáme `Supplier` třídy.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Tato třída se používá řetězec pro klíč entity. V praxi, který může být méně častých než použití klíče celé číslo. Ale je vhodné zobrazuje, jak OData zpracovává jiné typy klíčů kromě celých čísel.

V dalším kroku vytvoříme vztah přidáním `Supplier` vlastnost, která má `Product` třídy:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Přidejte nový **DbSet** k `ProductServiceContext` třídy, tak, aby rozhraní Entity Framework zahrne `Supplier` tabulky v databázi.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

V WebApiConfig.cs přidejte do modelu EDM entity "Dodavatelé":

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Navigační vlastnosti

Pokud chcete získat dodavatele pro produkt, klient odešle požadavek GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Tady je vlastnost navigace na "Dodavatel" `Product` typu. V takovém případě `Supplier` odkazuje na jednu položku, ale navigační vlastnost mohou vracet kolekci (vztah jeden mnoho nebo n: n).

Pro podporu této žádosti, přidejte následující metodu do `ProductsController` třídy:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

*Klíč* parametr je klíč produktu. Metoda vrátí související entity &#8212;v takovém případě `Supplier` instance. Název metody a název parametru jsou důležité. Obecně platí Pokud navigační vlastnost s názvem "X", budete muset přidat metodu s názvem "GetX". Metoda vyžaduje parametr s názvem "*klíč*" odpovídající datový typ klíče nadřazeného objektu.

Je také důležité zahrnout **[FromOdataUri]** atribut *klíč* parametr. Tento atribut určuje webového rozhraní API používat OData syntaxe pravidla v případě, že analyzuje klíč z identifikátoru URI požadavku.

## <a name="creating-and-deleting-links"></a>Vytváření a odstraňování odkazů

OData podporuje vytváření nebo odebírání vztahy mezi dvěma entitami. V terminologii OData relace je "link". Identifikátor URI s formuláři má každý odkaz *entity*/$links /*entity*. Odkaz z produktu dodavatele například vypadá takto:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Pokud chcete vytvořit nové propojení, klient odešle požadavek POST odkaz identifikátor URI. Text žádosti je identifikátor URI cílové entity. Předpokládejme například, že je dodavatele s klíčem "CTSO". Pokud chcete vytvořit odkaz ze "Produktu1" na "Supplier('CTSO')", klient odešle požadavek takto:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Pokud chcete odstranit odkaz, klient odešle žádost o odstranění propojení identifikátor URI.

**Vytváření odkazů**

Chcete-li klient k vytváření propojení produktu dodavatele, přidejte následující kód do `ProductsController` třídy:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Tato metoda přijímá tři parametry:

- *klíč*: klíč k nadřazená entita (produktu)
- *Vlastnost navigationProperty*: název navigační vlastnosti. V tomto příkladu je jediná platná navigační vlastnost "Dodavatel".
- *odkaz*: identifikátoru URI protokolu OData související entity. Tato hodnota jsou převzaty z textu požadavku. Například může být identifikátor URI na odkaz "`http://localhost/odata/Suppliers('CTSO')`, což znamená dodavatele s ID = 'CTSO'.

Metoda používá připojení k vyhledání dodavatele. Pokud je nalezen odpovídající dodavatele, metoda nastaví `Product.Supplier` vlastnost a výsledek uloží do databáze.

Nejtěžší část je analýza odkaz identifikátor URI. V podstatě budete muset simulovat výsledek odeslání požadavek GET na tento identifikátor URI. Následující pomocnou metodu ukazuje, jak to udělat. Metoda vyvolá proces směrování rozhraní Web API a získá zpět **ODataPath** instanci, která představuje Analyzovaná cesta OData. Pro odkaz identifikátor URI jedna segmenty by měla být klíč entity. (Pokud ne, klient zaslal chybný identifikátor URI.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**Odstraňování odkazů**

Chcete-li odstranit odkaz, přidejte následující kód do `ProductsController` třídy:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

V tomto příkladu je navigační vlastnost jednoho `Supplier` entity. Pokud je vlastnost navigace kolekce, identifikátor URI pro odstranění odkazu musí obsahovat klíč pro související entity. Příklad:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Tento požadavek odebere pořadí 1 zákazníka 1. V takovém případě bude mít metoda DeleteLink následující podpis:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

*RelatedKey* parametr poskytuje klíč pro související entity. Ano v vaše `DeleteLink` metoda vyhledávání primární entity podle *klíč* parametr najít související entity podle *relatedKey* parametr a potom odeberte přidružení. V závislosti na datový model, možná budete muset implementovat obě verze `DeleteLink`. Webové rozhraní API bude volat správnou verzi podle identifikátoru URI požadavku.
