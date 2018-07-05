---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Volání služby OData z klienta .NET (C#) | Dokumentace Microsoftu
author: MikeWasson
description: Tento kurz ukazuje postupy při volání služby OData z klientské aplikace C#. Verze softwaru, které jsou používané v kurzu Visual Studio 2013 (funguje s Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: d987e7fbe737055b3e2b690ef3e8de5ca7e2b937
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369780"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Volání služby OData z klienta .NET (C#)
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Tento kurz ukazuje postupy při volání služby OData z klientské aplikace C#.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funguje v sadě Visual Studio 2012)
> - [Klientská knihovna pro WCF Data Services](https://msdn.microsoft.com/library/cc668772.aspx)
> - Webové rozhraní API 2. (V příkladu služby OData se vytvořil pomocí webového rozhraní API 2, ale klientské aplikace nezávisí na webového rozhraní API.)


V tomto kurzu můžu projdete kroky vytvoření klientské aplikace, která volá ze služby OData. Služba OData zveřejňuje následující entity:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Následující články popisují, jak implementovat služby OData v rozhraní Web API. (Není nutné číst o tomto kurzu se ale.)

- [Vytváří se koncový bod OData ve webovém rozhraní API 2](creating-an-odata-endpoint.md)
- [Relace entit OData ve webovém rozhraní API 2](working-with-entity-relations.md)
- [Akce OData ve webovém rozhraní API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generování Proxy služby

Prvním krokem je generovat proxy služby. Proxy služby je třída rozhraní .NET, která definuje metody pro přístup ke službě OData. Proxy server překládá volání metod na požadavky HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Začněte tak, že otevřete projekt služby OData v aplikaci Visual Studio. Stisknutím kláves CTRL + F5 ke spuštění služby místně v rámci služby IIS Express. Poznámka: na místní adrese, včetně číslo portu, který přiřazuje sady Visual Studio. Tato adresa bude potřebovat při vytváření proxy serveru.

Dále otevřete jinou instanci sady Visual Studio a vytvořte projekt konzolové aplikace. Konzolová aplikace bude naše klientské aplikace OData. (Můžete také přidat projekt do stejného řešení jako službu.)

> [!NOTE]
> Zbývající kroky najdete v projektu konzoly.


V Průzkumníku řešení klikněte pravým tlačítkem na **odkazy** a vyberte **přidat odkaz na službu**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

V **přidat odkaz na službu** dialogové okno, zadejte adresu služby OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

kde *port* je číslo portu.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Pro **Namespace**, zadejte "ProductService". Tato možnost určuje obor názvů, třídy proxy.

Klikněte na tlačítko **Přejít**. Visual Studio načte dokument metadat OData ke zjišťování entit ve službě.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Klikněte na tlačítko **OK** do svého projektu přidat třídu proxy.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Vytvoření Instance třídy Proxy služby

Uvnitř vaší `Main` metodu, vytvořte novou instanci třídy proxy, následujícím způsobem:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Znovu použijte číslo skutečný port ve kterém je vaše služba spuštěná. Při nasazování služby budete používat identifikátor URI služby za provozu. Není nutné aktualizovat server proxy.

Následující kód přidá obslužnou rutinu události, která zobrazí žádost o identifikátory URI v okně konzoly. Tento krok není povinný, ale je zajímavé zobrazíte identifikátory URI pro každý dotaz.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Dotazování na službu

Následující kód načte seznam produktů ze služby OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Všimněte si, že nemusíte psát jakýkoli kód k odeslání požadavku HTTP a parsovat odpovědi. Proxy třída nemá tomto automaticky při vytvoření výčtu `Container.Products` kolekce **foreach** smyčky.

Když aplikaci spouštíte, výstup by měl vypadat nějak takto:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Chcete-li získat entity podle ID, použijte `where` klauzuli.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Pro zbývající část tohoto tématu, mohu nezobrazí celý `Main` fungovat, pouze kód, které jsou potřebné k vyvolání služby.

## <a name="apply-query-options"></a>Použije možnosti dotazu

OData definuje [možnosti dotazu](../supporting-odata-query-options.md) , který slouží k filtrování, řazení, data stránky a tak dále. V proxy služby můžete použít tyto možnosti s použitím různých LINQ – výrazy.

V této části ukážeme si stručný příklady. Další podrobnosti najdete v tématu [aspekty LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) na webové stránce MSDN.

### <a name="filtering-filter"></a>Filtrování ($filter)

Chcete-li filtrovat, použijte `where` klauzuli. Následující příklad filtry podle kategorie produktu.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Tento kód odpovídá následující dotaz OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Všimněte si, že se převede proxy serveru `where` klauzuli do OData `$filter` výrazu.

### <a name="sorting-orderby"></a>Řazení ($orderby)

Chcete-li seřadit, použijte `orderby` klauzuli. V následujícím příkladu se seřadí podle cena od nejvyšší po nejnižší.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Tady je odpovídající žádost OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Stránkování na straně klienta ($skip a $top)

Pro velká entita sady klient může chtít omezit počet výsledků. Klient může například zobrazit 10 položek najednou. Tento postup se nazývá *stránkování na straně klienta*. (K dispozici je také [stránkování na straně serveru](../supporting-odata-query-options.md#server-paging), kde server omezí počet výsledků.) Stránkování na straně klienta, použijte LINQ **přeskočit** a **trvat** metody. V následujícím příkladu Přeskočí prvních 40 výsledky a přijímá další 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Tady je odpovídající žádost OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Vyberte ($select) a rozbalení ($expand)

Chcete-li zahrnout související entity, použijte `DataServiceQuery<t>.Expand` metody. Například chcete zahrnout `Supplier` pro každou `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Tady je odpovídající žádost OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Chcete-li změnit tvar dané odpovědi, použití LINQ **vyberte** klauzuli. Následující příklad získá pouze název každého produktu se žádné vlastnosti.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Tady je odpovídající žádost OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Klauzule select může obsahovat související entity. V takovém případě Nevolejte **Rozbalit**; proxy server v tomto případě automaticky zahrnuje rozšíření. Následující příklad získá název a dodavatele výhod každého produktu.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Tady je odpovídající žádost OData. Všimněte si, že zahrnuje **$expand** možnost.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Další informace o $select a $expand rozbalte naleznete v tématu [pomocí $select $expand a $value ve webovém rozhraní API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Přidat novou entitu

Chcete-li přidat nové entity na sadu entit, zavolejte `AddToEntitySet`, kde *objektu EntitySet* je název sady entit. Například `AddToProducts` přidá nový `Product` k `Products` sady entit. Při generování proxy WCF Data Services automaticky vytvoří tyto silného typu **AddTo** metody.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Chcete-li přidat propojení mezi dvěma entitami, použijte **AddLink** a **SetLink** metody. Následující kód přidá nový dodavatele a nového produktu a vytvoří propojení mezi nimi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Použití **AddLink** po navigační vlastnost kolekce. V tomto příkladu přidáváme produkt, který má `Products` kolekce na dodavatele.

Použití **SetLink** po jedné entity navigační vlastnost. V tomto příkladu jsme nastavujete `Supplier` vlastnost na produktu.

## <a name="update--patch"></a>Aktualizovat nebo oprava

Chcete-li aktualizovat entitu, zavolejte **UpdateObject** metody.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Aktualizace se provádí při volání **SaveChanges**. Ve výchozím nastavení WCF odešle požadavek HTTP SLOUČENÍ. **PatchOnUpdate** přikazuje WCF místo odeslání HTTP PATCH.

> [!NOTE]
> Proč PATCH a MERGE? Původní specifikaci HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nedefinuje žádné metoda protokolu HTTP se sémantikou "částečné aktualizace". Specifikace prostředí OData pro podporu částečné aktualizace definované MERGE – metoda. V roce 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definované metodu PATCH pro částečné aktualizace. Si můžete přečíst některé z historie v tomto [blogový příspěvek](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) na blogu WCF Data Services. OPRAVA je v současné době upřednostňované nad SLOUČENÍ. Kontroler OData vytvořený generování uživatelského rozhraní webového rozhraní API podporuje obě metody.


Pokud mají být nahrazeny celou entity (PUT sémantiku), zadejte **ReplaceOnUpdate** možnost. To způsobí, že WCF odeslat požadavek HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Odstranit entitu

Chcete-li odstranit entitu, zavolejte **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Vyvolat akci OData

V prostředí OData [akce](odata-actions.md) představují způsob, jak přidat chování na straně serveru, které nejsou snadno definované jako operace CRUD u entit.

I když dokument metadat OData popisuje akce, třídy proxy nevytvoří žádné metody silného typu pro ně. Stále můžete vyvolat akci OData pomocí obecného **Execute** metody. Je ale potřeba znát datové typy parametrů a návratové hodnoty.

Například `RateProduct` akce přijímá parametr s názvem "Hodnocení" typu `Int32` a vrátí `double`. Následující kód ukazuje, jak vyvolat tuto akci.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Další informace najdete v tématu[volání operací služby a akce](https://msdn.microsoft.com/library/hh230677.aspx).

Jednou z možností je rozšířit **kontejneru** třídy silného typu metodu, která vyvolá akci:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
