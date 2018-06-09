---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Konvencí směrování v rozhraní ASP.NET Web API 2 Odata | Microsoft Docs
author: MikeWasson
description: Tento článek popisuje konvencí směrování, která webového rozhraní API používá koncových bodů protokolu OData.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28037318"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Konvencí směrování v rozhraní ASP.NET Web API 2 Odata
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Tento článek popisuje konvencí směrování, která webového rozhraní API používá koncových bodů protokolu OData.


Když webového rozhraní API získá požadavek OData, mapuje žádost názvu kontroleru a název akce. Mapování je založena na metodu protokolu HTTP a identifikátor URI. Například `GET /odata/Products(1)` mapuje `ProductsController.GetProduct`.

V tomto článku část 1 I popisují předdefinované konvencí směrování protokolu OData. Tyto konvence byly navrženy specificky pro koncových bodů protokolu OData a nahrazují výchozí směrování systém webového rozhraní API. (Nahrazení se stane, když zavoláte **MapODataRoute**.)

V části 2 I ukazují, jak přidat vlastní konvencí směrování. Aktuálně se předdefinované názvů nepokrývají celý rozsah OData pro identifikátory URI, ale můžete rozšířit, aby zpracovat další případy.

- [Předdefinované konvencí směrování](#conventions)
- [Vlastní konvencí směrování](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Předdefinované konvencí směrování

Před I popisují konvencí směrování protokolu OData v rozhraní Web API, je vhodné se seznámit s identifikátory URI protokolu OData. [Identifikátoru URI protokolu OData](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) se skládá z:

- Kořenový adresář
- Cesta prostředku
- Možnosti dotazu

![](odata-routing-conventions/_static/image1.png)

Pro směrování, je důležitou součástí cesta prostředku. Cesta prostředku je rozdělené do segmentů. Například `/Products(1)/Supplier` má tři segmenty:

- `Products` odkazuje na sadu entit s názvem "Produktů".
- `1` je klíč entity, výběrem jedné entity ze sady.
- `Supplier` je navigační vlastnost, která vybere související entity.

Tuto cestu, vybere se dodavatele produkt 1.

> [!NOTE]
> Segmenty cesty OData nemusí vždy odpovídat segmenty identifikátor URI. Například "1" je považován za segmentu cesty.


**Názvy řadiče.** Název kontroleru je vždy odvozená od entity, nastavte v kořenovém adresáři cesta prostředku. Například, pokud cesta prostředku je `/Products(1)/Supplier`, webového rozhraní API hledá řadič s názvem `ProductsController`.

**Názvy akcí.** Názvy akce jsou odvozeny od segmenty cesty a datového modelu entity (EDM), jak je uvedeno v následujících tabulkách. V některých případech máte dvě možnosti pro název akce. Například "Get" nebo &quot;GetProducts&quot;.

**Dotazování entity**

| Požadavek | Příklad identifikátoru URI | Název akce | Příklad akce |
| --- | --- | --- | --- |
| ZÍSKAT /entityset | / Produkty | GetEntitySet nebo Get | GetProducts |
| ZÍSKAT /entityset(key) | /Products(1) | GetEntityType nebo Get | GetProduct |
| ZÍSKAT /entityset (klíč) / přetypování | / /Models.Book produkty (1) | GetEntityType nebo Get | GetBook |

Další informace najdete v tématu [vytvořit koncový bod OData jen pro čtení](odata-v3/creating-an-odata-endpoint.md).

**Vytváření, aktualizaci a odstraňování entit**

| Požadavek | Příklad identifikátoru URI | Název akce | Příklad akce |
| --- | --- | --- | --- |
| POST /entityset | / Produkty | PostEntityType nebo Post | PostProduct |
| UVEĎTE /entityset(key) | /Products(1) | PutEntityType nebo Put | PutProduct |
| UVEĎTE /entityset (klíč) / přetypování | / /Models.Book produkty (1) | PutEntityType nebo Put | PutBook |
| Oprava /entityset(key) | /Products(1) | PatchEntityType nebo oprava | PatchProduct |
| Oprava /entityset (klíč) / přetypování | / /Models.Book produkty (1) | PatchEntityType nebo oprava | PatchBook |
| Odstranit /entityset(key) | /Products(1) | DeleteEntityType nebo odstranění | DeleteProduct |
| Odstranit /entityset (klíč) / přetypování | / /Models.Book produkty (1) | DeleteEntityType nebo odstranění | DeleteBook |

**Dotazování na navigační vlastnost**

| Požadavek | Příklad identifikátoru URI | Název akce | Příklad akce |
| --- | --- | --- | --- |
| GET /entityset (klíč) nebo navigace | / Produkty (1) nebo dodavatele | GetNavigationFromEntityType nebo GetNavigation | GetSupplierFromProduct |
| ZÍSKAT /entityset (klíč) / přetypování/navigace | / /Models.Book/Author produkty (1) | GetNavigationFromEntityType nebo GetNavigation | GetAuthorFromBook |

Další informace najdete v tématu [práce se vztahy entit](odata-v3/working-with-entity-relations.md).

**Vytváření a odstraňování odkazů**

| Požadavek | Příklad identifikátoru URI | Název akce |
| --- | --- | --- |
| /Entityset POST (klíč) nebo $links/navigace | / (1) / $produkty odkazy nebo dodavatele | CreateLink |
| Vložení /entityset (klíč) nebo $links/navigace | / (1) / $produkty odkazy nebo dodavatele | CreateLink |
| ODSTRANĚNÍ /entityset (klíč) nebo $links/navigace | / (1) / $produkty odkazy nebo dodavatele | DeleteLink |
| Odstranit /entityset(key)/$links/navigation(relatedKey) | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Další informace najdete v tématu [práce se vztahy entit](odata-v3/working-with-entity-relations.md).

**Vlastnosti**

*Vyžaduje rozhraní Web API 2*

| Požadavek | Příklad identifikátoru URI | Název akce | Příklad akce |
| --- | --- | --- | --- |
| GET /entityset (klíč) nebo vlastnost | / Produkty (1) nebo název | GetPropertyFromEntityType nebo GetProperty – | GetNameFromProduct |
| ZÍSKAT /entityset (klíč) nebo přetypování nebo vlastnost | / /Models.Book/Author produkty (1) | GetPropertyFromEntityType nebo GetProperty – | GetTitleFromBook |

**Akce**

| Požadavek | Příklad identifikátoru URI | Název akce | Příklad akce |
| --- | --- | --- | --- |
| /Entityset POST (klíč) nebo akce | / Produkty (1) nebo míry | ActionNameOnEntityType nebo název akce | RateOnProduct |
| POST /entityset (klíč) nebo přetypování nebo akce | / /Models.Book/CheckOut produkty (1) | ActionNameOnEntityType nebo název akce | CheckOutOnBook |

Další informace najdete v tématu [akcí OData](odata-v3/odata-actions.md).

**Podpisy – metoda**

Tady jsou některá pravidla pro podpisy metod:

- Pokud cesta obsahuje klíč, akce musí mít parametr s názvem *klíč*.
- Pokud cesta obsahuje klíč do navigační vlastnost, akce musí mít parametr s názvem *relatedKey*.
- Uspořádání *klíč* a *relatedKey* parametry se **[FromODataUri]** parametr.
- POST a PUT požadavky trvat parametr typu entity.
- Oprava požadavky přebírají parametr typu **rozdílů&lt;T&gt;**, kde *T* je typ entity.

Pro referenci tady je příklad, který ukazuje metoda podpisy pro každé předdefinované konvencí směrování OData.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Vlastní konvencí směrování

Předdefinované konvence aktuálně nepokrývají všechny možné identifikátory URI protokolu OData. Můžete přidat nové konvence implementací **IODataRoutingConvention** rozhraní. Toto rozhraní se dvě metody:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** vrací název řadiče.
- **SelectAction** vrátí název akce.

Pro obě metody Pokud konvence se nevztahuje na tuto žádost, metoda by měla vrátit hodnotu null.

**ODataPath** parametr představuje Analyzovaná cesta prostředku OData. Obsahuje seznam **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** instance, jednu pro každý segment cesty prostředku. **ODataPathSegment** je abstraktní třída; každý typ segmentu je reprezentována třídu odvozenou z **ODataPathSegment**.

**ODataPath.TemplatePath** vlastnost je řetězec, který představuje zřetězení všechny segmenty cesty. Například pokud je identifikátor URI `/Products(1)/Supplier`, je šablona cesty &quot;~/entityset/key/navigation&quot;. Všimněte si, zda není přímo na URI segmenty odpovídají segmentů. Například klíč entity (1) je reprezentován jako vlastní **ODataPathSegment**.

Obvykle implementace **IODataRoutingConvention** provede následující akce:

1. Porovnejte šablonu cesty a zjistěte, zda tato konvence platí pro aktuální požadavek. Pokud nevztahuje, vrátí hodnotu null.
2. Pokud se vztahuje konvence, použijte vlastnosti **ODataPathSegment** instance k odvození názvu kontroleru a akce.
3. U akcí přidejte všechny hodnoty do slovníku trasy, která by měla vytvořit vazbu k parametrům akce (obvykle klíče entity).

Podívejme se na konkrétní příklad. Předdefinované konvencí směrování nepodporují indexování do kolekce navigace. Jinými slovy neexistuje žádný konvence pro identifikátory URI jako následující:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Zde je konvenci vlastní směrování pro tento typ dotazu zpracování.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Poznámky:

1. Jsou odvozeny od **EntitySetRoutingConvention**, protože **SelectController** metody v dané třídě je vhodný pro tento nový konvencí směrování. To znamená, že nemusíte znovu implementovat **SelectController**.
2. Konvence platí jenom pro požadavky GET, a jenom v případě, že je šablona cesty &quot;~/entityset/key/navigation/key&quot;.
3. Je název akce &quot;získat {EntityType}&quot;, kde *{EntityType}* je typem navigace kolekce. Například &quot;GetSupplier&quot;. Můžete použít všechny zásady vytváření názvů, který chcete &#8212; právě zajistěte, aby vaše akce kontroleru odpovídají.
4. Akce přebírá dva parametry s názvem *klíč* a *relatedKey*. (Seznam některé názvy předdefinované parametrů najdete v tématu [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Dalším krokem je přidání nové konvence do seznamu konvencí směrování. K tomu dojde během konfigurace, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

Zde jsou některé další ukázkové konvencí směrování které být užitečné pro zkoumání:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

A samozřejmě webového rozhraní API, samotné je typu open source, abyste viděli [zdrojový kód](http://aspnetwebstack.codeplex.com/) pro předdefinované konvencí směrování. Tyto jsou definované v **System.Web.Http.OData.Routing.Conventions** oboru názvů.
