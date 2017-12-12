---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: "Vztahy entit v OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Microsoft Docs"
author: MikeWasson
description: "Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy mít autoři; produkty mít dodavatelů. Použití protokolu OData, klienti můžete přejít přes..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Vztahy entit v OData v4 pomocí rozhraní ASP.NET Web API 2.2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Většina datových sad definovat vztahy mezi entitami: Zákazníci mají objednávky; knihy mít autoři; produkty mít dodavatelů. Použití protokolu OData, klienti můžete přejít přes vztahy entit. Zadaný produkt, můžete najít dodavatele. Můžete také vytvářet nebo odebírat vztahy. Například můžete nastavit dodavatele pro produkt.
> 
> Tento kurz ukazuje, jak pro podporu těchto operací OData v4, pomocí rozhraní ASP.NET Web API. Tento kurz je založený na kurz [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md).
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
> OData verze 3, najdete v části [podpora vztahy entit v OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Přidání Entity dodavatele

> [!NOTE]
> Tento kurz je založený na kurz [vytvoření OData v4 koncový bod pomocí rozhraní ASP.NET Web API 2](create-an-odata-v4-endpoint.md).


Nejdřív potřebujeme související entity. Přidejte třídu s názvem `Supplier` ve složce modelů.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Přidat navigační vlastnost, která má `Product` třídy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Přidejte nový **DbSet** k `ProductsContext` třídy, tak, aby rozhraní Entity Framework bude obsahovat dodavatele tabulky v databázi.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

V WebApiConfig.cs, přidejte &quot;Dodavatelé&quot; sada entit do datového modelu entity:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Přidat řadič dodavatelů

Přidat `SuppliersController` třídy ke složce řadiče.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

I nebude ukazují, jak přidat operace CRUD pro tento kontroler. Kroky jsou stejné jako pro řadič produkty (viz [vytvořit koncový bod OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Získávání entit v relaci

Pokud chcete získat dodavatele pro produkt, klient odešle požadavek GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Pro podporu této žádosti, přidejte následující metodu do `ProductsController` třídy:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Tato metoda používá výchozí zásady vytváření názvů

- Název metody: GetX, kde X je navigační vlastnost.
- Název parametru: *klíč*

Pokud budete postupovat podle tyto zásady vytváření názvů, webového rozhraní API automaticky mapuje požadavek HTTP metodu řadiče.

Příklad protokolu HTTP žádosti:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Příklad HTTP odpovědi:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Získání související kolekce

V předchozím příkladu má produkt jednoho dodavatele. Vlastnost navigace mohou vracet kolekci. Následující kód získá produkty pro dodavatele:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

V takovém případě metoda vrátí **IQueryable** místo **SingleResult&lt;T&gt;**

Příklad protokolu HTTP žádosti:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Příklad HTTP odpovědi:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Vytvoření vztahu mezi entitami

OData podporuje vytváření nebo odebírání vztahy mezi dvěma entitami existující. V terminologii OData v4, je relace &quot;odkaz&quot;. (V OData v3, byla volána relace *odkaz*. Rozdíly protokolu nejsou důležité pro účely tohoto kurzu.)

Odkaz má svou vlastní identifikátor URI s formuláři `/Entity/NavigationProperty/$ref`. Zde je ukázka, identifikátor URI k vyřešení odkaz mezi produkt a jeho dodavatele:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Chcete-li přidat relaci, klient odešle požadavek POST nebo PUT na tuto adresu.

- Pokud navigační vlastnost je jedna entita, jako například `Product.Supplier`.
- POST, pokud navigační vlastnost kolekce, jako například `Supplier.Products`.

Text žádosti obsahuje identifikátor URI entity v vztah. Tady je příklad požadavek:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

V tomto příkladu, klient odešle požadavek PUT pro `/Products(6)/Supplier/$ref`, což je identifikátor URI $ref `Supplier` produktu s ID = 6. Pokud neproběhne úspěšně, server odešle 204 odpovědi (ne obsahu):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Tady je metoda kontroleru přidat vztah k `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

*NavigationProperty* parametr určuje, které vztah k nastavení. (Pokud existuje více než jednu navigační vlastnost u entity, můžete přidat více `case` příkazy.)

*Odkaz* parametr obsahuje identifikátor URI dodavatele. Webové rozhraní API automaticky analyzuje text požadavku má být získána hodnota tohoto parametru.

K vyhledání dodavatele, potřebujeme ID (nebo klíče), který je součástí *odkaz* parametr. K tomu použijte následující pomocnou metodu:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

V podstatě tato metoda používá knihovnou OData rozdělit cesta k identifikátoru URI na segmenty, najděte segment, který obsahuje klíč a převést klíč do správného typu.

## <a name="deleting-a-relationship-between-entities"></a>Odstranění vztahu mezi entitami

Pokud chcete odstranit relaci, klient odešle žádost HTTP DELETE k identifikátoru URI $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Tady je metoda kontroleru odstranit vztah mezi produktu a dodavatele:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

V takovém případě `Product.Supplier` je &quot;1&quot; konec relace 1: n, tak odeberete vztah právě nastavením `Product.Supplier` k `null`.

V &quot;mnoho&quot; konci relace, klient musíte zadat, která relaci entity k odebrání. Uděláte to tak, pošle klient identifikátor URI entity v relaci v řetězci dotazu požadavku. Chcete-li například odebrat "Produkt 1" z "dodavatele 1":

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

Pro podporu tohoto v rozhraní Web API, musíme zahrnout další parametr v `DeleteRef` metoda. Tady je metoda kontroleru odebrat z produktu `Supplier.Products` vztah.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

*Klíč* parametr je klíč pro dodavatele a *relatedKey* parametr je klíč pro odebrání z produktu `Products` vztah. Všimněte si, že webového rozhraní API automaticky získá klíč z řetězce dotazu.
