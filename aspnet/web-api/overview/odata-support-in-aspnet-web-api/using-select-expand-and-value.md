---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: "Pomocí $select, $, rozbalte položku a $value v prostředí ASP.NET Web API 2 OData | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a>Pomocí $select, $, rozbalte položku a $value v prostředí ASP.NET Web API 2 OData
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Webové rozhraní API 2 přidává podporu pro rozšířit $expand, $select a možnosti $value protokolu OData. Tyto možnosti Povolit klienta k řízení reprezentaci, který získá zpět ze serveru.

- **Rozbalte $** způsobí, že mají být zahrnuty vložené v odpovědi související entity.
- **$select** vybere podmnožinu vlastnosti, které chcete zahrnout do odpovědi.
- **$value** získá nezpracovanou hodnotu vlastnosti.

## <a name="example-schema"></a>Příklad schématu

V tomto článku budete použít služby OData, která definuje tři entity: produktu, dodavatel a kategorie. Každý produkt má jednu kategorii a jednoho dodavatele.

![](using-select-expand-and-value/_static/image1.png)

Zde jsou tříd jazyka C#, které definují modelem entity:

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

Všimněte si, že `Product` třída definuje navigační vlastnosti pro `Supplier` a `Category`. `Category` Třída definuje navigační vlastnost pro produkty v každé kategorii.

Chcete-li vytvořit koncový bod OData pro toto schéma, použijte generování uživatelského rozhraní sady Visual Studio 2013, jak je popsáno v [vytváření koncový bod OData v rozhraní ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md). Přidáte samostatné řadiče pro produkt, kategorie a dodavatele.

## <a name="enabling-expand-and-select"></a>Povolení $rozbalte a $select

Ve Visual Studiu 2013 se vytvoří generování uživatelského rozhraní Web API OData řadič této automaticky podporuje $expand a $select. Pro referenci tady jsou požadavky na podporu $rozbalte a $select v kontroleru.

Pro kolekce, řadičem na `Get` metoda musí vrátit **IQueryable**.

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

Pro jednotlivé entity vrátit **SingleResult&lt;T&gt;**, kde T představuje **IQueryable** obsahující žádnou nebo jednu entitu.

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

Navíc uspořádání vaší `Get` metody s **[Queryable]** atributu, jak je znázorněno v předchozích fragmentů kódu. Alternativně volání **EnableQuerySupport** na **HttpConfiguration** objekt při spuštění. (Další informace najdete v tématu [povolení možnosti dotazu OData](supporting-odata-query-options.md#enable).)

## <a name="using-expand"></a>Pomocí $rozbalte

Při dotazu OData entity nebo kolekci, výchozí odpověď neobsahuje entit v relaci. Zde je ukázka, výchozí odpověď pro sadu entit kategorií:

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

Jak vidíte, odpověď neobsahuje všechny produkty, i když kategorie entity má navigační odkaz produkty. Klient však můžete použít $rozbalte získat seznam produktů pro každou kategorii. Rozbalte možnost $expand přejde v řetězci dotazu požadavku:

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

Server bude nyní zahrnují tyto produkty pro každou kategorii vložením s kategorií. Tady je datové části odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

Všimněte si, že každou položku v poli "value" obsahuje seznam produktů.

$Expand rozbalte možnost trvá čárkami oddělený seznam navigačních vlastností pro rozbalení. Následující požadavek rozšíří kategorii a dodavatel pro produkt.

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

Můžete rozbalit více než jednu úroveň navigační vlastnosti. Následující příklad obsahuje všechny produkty pro kategorii a také dodavatel pro každý produkt.

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

Ve výchozím omezení webového rozhraní API rozšíření maximální hloubka 2. Který brání klientovi odesílání komplexní požadavků jako `$expand=Orders/OrderDetails/Product/Supplier/Region`, což může být neefektivní pro dotazování a vytvořit velké odpovědi. Chcete-li přepsat výchozí nastavení, nastavte **MaxExpansionDepth** vlastnost **[Queryable]** atribut.

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

Další informace o $rozšířit možnost, najdete v části [rozbalte možností dotazu systému ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) v oficiální dokumentaci OData.

## <a name="using-select"></a>Pomocí $select

Možnost $select určuje podmnožinu vlastnosti, které chcete zahrnout do text odpovědi. Například získat jenom název a cena jednotlivých produktů, použijte následující dotaz:

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

Můžete kombinovat $select a $expand rozbalte ve stejném dotazu. Nezapomeňte zahrnout vlastnost rozšířené možnosti $select. Například následující požadavek získá název produktu a dodavatele.

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

Můžete také vybrat vlastnosti v rámci rozšířené vlastnosti. Následující žádost o rozšíří produkty a vybere název kategorie a název produktu.

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

Tady je text odpovědi:

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

Další informace o možnost $select najdete v tématu [vyberte možností dotazu systému ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) v oficiální dokumentaci OData.

## <a name="getting-individual-properties-of-an-entity-value"></a>Získávání jednotlivé vlastnosti entity ($value)

Existují dva způsoby pro klienta OData k získání jednotlivých vlastnost z entity. Klienta můžete získat hodnotu ve formátu OData nebo získá nezpracovanou hodnotu vlastnosti.

Následující požadavek získá vlastnost ve formátu OData.

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

Tady je příklad odpověď ve formátu JSON:

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

Chcete-li získat nezpracovanou hodnotu vlastnosti, připojte $value identifikátor URI:

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

Zde je odpovědi. Všimněte si, že typ obsahu, který není "text/plain" JSON.

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

Pro podporu těchto dotazů do kontroleru OData, přidejte metodu s názvem `GetProperty`, kde `Property` je název vlastnosti. Například by s názvem metodu GET pro vlastnost název `GetName`. Metoda by měla vrátit hodnotu této vlastnosti:

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
