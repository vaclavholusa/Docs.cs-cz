---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Pomocí $select $expand a $value v ASP.NET Web API 2 OData | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/11/2013
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: d198ecf40155cba36204bc0810f4735aae6b100b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751836"
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Pomocí $select $expand a $value v ASP.NET Web API 2 OData
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Webové rozhraní API 2 přidává podporu pro $$expand, $select a možnosti $value prostředí OData. Tyto možnosti umožňují klienta pro řízení reprezentaci, který získá zpět ze serveru.

- **$expand** způsobí, že související entity, které být zahrnuty vložené v odpovědi.
- **$select** vybere podmnožinu vlastnosti, které chcete zahrnout do odpovědi.
- **$value** získá nezpracovanou hodnotu vlastnosti.

## <a name="example-schema"></a>Příklad schématu

Pro účely tohoto článku použiju služby OData, který definuje tři entity: produktu, dodavatel a kategorie. Každý produkt má jednu kategorii a jednoho dodavatele.

![](using-select-expand-and-value/_static/image1.png)

Tady jsou třídy C#, které definují modely entity:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Všimněte si, že `Product` třídy definuje vlastnosti navigace `Supplier` a `Category`. `Category` Třída definuje vlastnost navigace pro produkty v jednotlivých kategoriích.

Chcete-li vytvořit koncový bod OData pro toto schéma, použijte generování uživatelského rozhraní sady Visual Studio 2013, jak je popsáno v [vytváření koncového bodu OData v rozhraní ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md). Přidejte samostatný řadiče pro produkt, kategorie a dodavateli.

## <a name="enabling-expand-and-select"></a>Povolení $rozbalte a $select

V sadě Visual Studio 2013 generování uživatelského rozhraní Web API OData vytvoří kontroler, který automaticky podporuje $expand a $select. Pro srovnání, tady jsou požadavky na podporu $rozbalte a $select v kontroleru.

Pro kolekce, kontroler společnosti `Get` metoda musí vracet **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Pro jednotlivé entity, vrátí **SingleResult&lt;T&gt;**, kde T je **IQueryable** obsahující žádnou nebo jednu entitu.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Navíc uspořádání vašich `Get` metody s **[Queryable]** atributu, jak je znázorněno v předchozích fragmentů kódu. Můžete také volat **EnableQuerySupport** na **HttpConfiguration** při spuštění. (Další informace najdete v tématu [povolení možnosti dotazu OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Rozbalte položku pomocí $

Když odešlete dotaz OData entitu nebo kolekci, výchozí odpověď neobsahuje související entity. Tady je příklad, výchozí odpověď pro sadu entit kategorií:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Jak je vidět, odpověď neobsahuje žádné produkty, i v případě, že má entita kategorie produktů navigační odkaz. Však může klient použít $rozbalte zobrazíte seznam produktů pro každou kategorii. Možnost rozbalování na $ přejde v řetězci dotazu požadavku:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Server teď bude obsahovat produktů pro každou kategorii, vložený v kategorii. Tady je datové části odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Všimněte si, že každá položka v poli "value" obsahuje seznam produktů.

$Expand možnost přijímá čárkami oddělený seznam navigačních vlastností pro rozbalení. Následující požadavek rozšíří kategorii a dodavatele, produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Můžete rozbalit více než jednu úroveň navigační vlastnosti. Následující příklad obsahuje všechny produkty pro kategorii a také dodavatel pro jednotlivé produkty.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Ve výchozím omezení webového rozhraní API hloubku maximální rozšíření na 2. Klient, který brání v odesílání složité požadavky, jako je `$expand=Orders/OrderDetails/Product/Supplier/Region`, který může být neefektivní pro dotazování a vytvořit velké odpovědi. Chcete-li přepsat výchozí hodnotu, nastavte **MaxExpansionDepth** vlastnost na **[Queryable]** atribut.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Další informace o $rozšířit možnosti, najdete v části [rozbalte možností dotazu systému ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) v oficiální dokumentaci OData.

## <a name="using-select"></a>Pomocí $select

Možnost $select určuje podmnožinu vlastností, které budou obsahovat v textu odpovědi. Například pokud chcete získat název a ceny jednotlivých produktů, použijte následující dotaz:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Můžete kombinovat $select a $expand ve stejném dotazu. Ujistěte se, že mají být zahrnuty vlastnost rozšířené možnosti $select. Například následující požadavek získá název produktu a dodavateli.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Můžete také vybrat vlastnosti v rámci rozšířené vlastnosti. Následující požadavek rozšiřuje produktů a vybere název kategorie a název produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Další informace o možnosti $select, naleznete v tématu [vyberte možností dotazu systému ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) v oficiální dokumentaci OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Načtení jednotlivých vlastností entity ($value)

Existují dva způsoby, jak klient OData Chcete-li získat jednotlivé vlastnost z entity. Klienta můžete získat hodnotu ve formátu OData nebo získat nezpracované hodnoty vlastnosti.

Následující požadavek získá vlastnost ve formátu OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Tady je příklad odpovědi ve formátu JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Nezpracovaná hodnota vlastnosti získáte připojte k identifikátoru URI $value:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Tady je tato odpověď. Všimněte si, že typ obsahu není "text/plain" JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Pro podporu těchto dotazů v řadiče OData, přidejte metodu s názvem `GetProperty`, kde `Property` je název vlastnosti. Například metoda GET pro vlastnost název pojmenován `GetName`. Metoda by měla vrátit hodnota dané vlastnosti:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
