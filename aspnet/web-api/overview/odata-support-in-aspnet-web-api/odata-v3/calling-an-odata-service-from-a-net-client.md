---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: "Volání služby OData z klienta .NET (C#) | Microsoft Docs"
author: MikeWasson
description: "Tento kurz ukazuje způsob volání služby OData z klientské aplikace C#. Verze softwaru, které jsou používány kurzu Visual Studio 2013 (funguje s Visual S..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Volání služby OData z klienta .NET (C#)
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Tento kurz ukazuje způsob volání služby OData z klientské aplikace C#.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funguje s Visual Studio 2012)
> - [Klientská knihovna pro WCF Data Services](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2. (V příkladu služby OData je sestaven pomocí webového rozhraní API 2, ale klientské aplikace není závislá na webové rozhraní API.)


V tomto kurzu budete I provede procesem vytvoření klientskou aplikaci, která volá služby OData. Služba OData zveřejňuje následující entity:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Na následující články popisují, jak implementovat službu OData v rozhraní Web API. (Není potřeba je pochopit tohoto kurzu, ale přečíst.)

- [Vytváření koncový bod OData v rozhraní Web API 2](creating-an-odata-endpoint.md)
- [Vztahy entit OData v rozhraní Web API 2](working-with-entity-relations.md)
- [Akce OData ve webovém rozhraní API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generovat Proxy služby

Prvním krokem je ke generování proxy služby. Proxy server služby je třída rozhraní .NET, která definuje metody pro přístup ke službě OData. Proxy server přeloží volání metod na požadavky HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Začněte otevřením projektu služby OData v sadě Visual Studio. Stisknutím CTRL + F5 a spusťte službu lokálně v IIS Express. Všimněte si místní adresy, včetně číslo portu, který přiřazuje Visual Studio. Tuto adresu budete potřebovat při vytváření proxy serveru.

Dále otevřete jiná instance sady Visual Studio a vytvořte projekt konzolové aplikace. Konzolové aplikace bude naše OData klientské aplikace. (Můžete také přidat projekt do stejného řešení, jako služba.)

> [!NOTE]
> Příklady zbývajících kroků najdete v konzolovém projektu.


V Průzkumníku řešení klikněte pravým tlačítkem na **odkazy** a vyberte **přidat odkaz na službu**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

V **přidat odkaz na službu** dialogové okno, zadejte adresu služby OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

kde *portu* je číslo portu.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Pro **Namespace**, zadejte "ProductService". Tato možnost definuje obor názvů, třídy proxy.

Klikněte na tlačítko **přejděte**. Visual Studio přečte dokument metadat OData ke zjišťování entit ve službě.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Klikněte na tlačítko **OK** k do projektu přidejte třídu proxy.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Vytvoření Instance třídy Proxy služby

Uvnitř vaší `Main` metoda, vytvořte novou instanci třídy proxy, následujícím způsobem:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Znovu použijte skutečný port číslo se spuštěným služby. Při nasazení služby, použijete identifikátor URI služby za provozu. Nemusíte aktualizovat proxy serveru.

Následující kód přidá obslužnou rutinu události, který zobrazí žádost o identifikátory URI v okně konzoly. Tento krok není povinné, ale je zajímavé najdete v části identifikátory URI pro každý dotaz.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Zadat dotaz na službu

Následující kód získá seznam produktů od služby OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Všimněte si, že nemusíte psaní jakéhokoli kódu k odeslání požadavku HTTP nebo analyzovat odpověď. Neobsahuje třídu proxy tomto automaticky při vytvoření výčtu `Container.Products` kolekce **foreach** smyčky.

Při spuštění aplikace výstup by měl vypadat asi takto:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

K načtení entity podle ID, použijte `where` klauzule.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Pro zbývající část tohoto tématu, nebude zobrazit celý `Main` fungovat pouze kód potřebných k volání služby.

## <a name="apply-query-options"></a>Použije možnosti dotazu

Definuje OData [možnosti dotazu](../supporting-odata-query-options.md) který slouží k filtrování, řazení, data stránky a tak dále. V proxy služby můžete použít tyto možnosti s využitím různých LINQ – výrazy.

V této části budete zobrazit stručný příklady. Další podrobnosti naleznete v tématu [aspekty LINQ (služby WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) na webu MSDN.

### <a name="filtering-filter"></a>Filtrování ($filter)

Chcete-li filtrovat, použijte `where` klauzule. Následující příklad filtry podle kategorie produktu.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Tento kód odpovídá následujícího dotazu OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Všimněte si, že proxy převede `where` klauzule do OData `$filter` výraz.

### <a name="sorting-orderby"></a>Řazení ($orderby)

Chcete-li seřadit, použijte `orderby` klauzule. V následujícím příkladu se seřadí podle ceny od nejvyšší po nejnižší.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Zde je odpovídající žádost OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Klientské stránkování ($skip a $top)

Pro velké entity sady klient může chtít omezit počet výsledků. Klient může například zobrazit 10 položek najednou. To se označuje jako *stránkování na straně klienta*. (K dispozici je také [stránkování na straně serveru](../supporting-odata-query-options.md#server-paging), kde server omezí počet výsledků.) Pokud chcete provést stránkování na straně klienta, použijte LINQ **přeskočit** a **trvat** metody. V následujícím příkladu přeskočí první 40 výsledky a provede další 10.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Tady je odpovídající žádost OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Vyberte ($select) a rozbalte ($expand)

Chcete-li zahrnout entit v relaci, použijte `DataServiceQuery<t>.Expand` metoda. Chcete-li například zahrnout `Supplier` pro každou `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Tady je odpovídající žádost OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Obrazec odpovědi, můžete změnit LINQ **vyberte** klauzule. Následující příklad získá název jednotlivých produktů, bez jakýchkoli dalších vlastností.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Tady je odpovídající žádost OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Klauzuli select může zahrnovat entit v relaci. V takovém případě Nevolejte **rozbalte**; proxy serveru v takovém případě automaticky zahrne rozšíření. Následující příklad načte název a dodavatele každého produktu.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Zde je odpovídající žádost OData. Všimněte si, že obsahuje **$expand** možnost.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Další informace o $select a $expand rozbalte, najdete v části [pomocí $select, $, rozbalte položku a $value ve webovém rozhraní API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Přidat novou entitu

Chcete-li přidat nové entity na sadu entit, volejte `AddToEntitySet`, kde *EntitySet* je název sady entit. Například `AddToProducts` přidá nový `Product` k `Products` sady entit. Při generování proxy služby WCF Data Services automaticky vytvoří tyto silného typu **AddTo** metody.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Chcete-li přidat odkaz mezi dvěma entitami, použijte **AddLink** a **SetLink** metody. Následující kód přidá nové dodavatele a nového produktu a poté vytvoří propojení mezi nimi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Použití **AddLink** po navigační vlastnost kolekce. V tomto příkladu přidáváme produkt, který `Products` kolekce na dodavatele.

Použití **SetLink** po jedné entity navigační vlastnost. V tomto příkladu jsme nastavení `Supplier` vlastnost na produktu.

## <a name="update--patch"></a>Aktualizovat / oprava

Chcete-li aktualizovat entitu, volejte **UpdateObject** metoda.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Aktualizace se provádí při volání **SaveChanges**. Ve výchozím nastavení WCF odešle požadavek HTTP SLOUČENÍ. **PatchOnUpdate** možnost informuje WCF místo toho odeslat HTTP PATCH.

> [!NOTE]
> Proč PATCH a MERGE? Původní specifikace protokolu HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) nebyly definovány žádné metoda HTTP s sémantiku "částečné aktualizace". Specifikace prostředí OData pro podporu částečné aktualizace, definována metodu SLOUČENÍ. V 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definované metodu PATCH pro částečné aktualizace. Si můžete přečíst některé z historie v tomto [příspěvku na blogu](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) na blogu WCF Data Services. OPRAVA je v současné době upřednostňované nad SLOUČENÍ. Kontroler OData vytvořený generováním uživatelského rozhraní Web API podporuje obě metody.


Pokud chcete nahradit celý entity (PUT sémantiku), zadejte **ReplaceOnUpdate** možnost. To způsobí, že WCF odeslat požadavek HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Odstranění Entity

Pokud chcete odstranit entitu, volejte **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Vyvolání akce OData

V prostředí OData [akce](odata-actions.md) představují způsob, jak přidat serverové chování, které nejsou snadno definován jako operace CRUD na entity.

I když dokument metadat OData popisuje akce, třídu proxy nevytvoří žádné metody silného typu pro ně. Přesto můžete vyvolat akci OData pomocí Obecné **Execute** metoda. Ale musíte vědět, datové typy parametrů a návratovou hodnotu.

Například `RateProduct` akce má parametr s názvem "Hodnocení" typu `Int32` a vrátí `double`. Následující kód ukazuje, jak má být vyvolán tuto akci.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Další informace najdete v tématu[volání operací služby a akce](https://msdn.microsoft.com/library/hh230677.aspx).

Jednou z možností je rozšířit **kontejneru** třída zajistit silného typu metodu, která vyvolá akci:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
