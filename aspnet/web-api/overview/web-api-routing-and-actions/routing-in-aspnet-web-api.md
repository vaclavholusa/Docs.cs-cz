---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Směrování ve webovém rozhraní API technologie ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/11/2012
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 458f9a6369fe97bab33d70bf31bd470b1b0e593c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756420"
---
<a name="routing-in-aspnet-web-api"></a>Směrování v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Tento článek popisuje, jak rozhraní ASP.NET Web API směruje požadavky HTTP na řadiče.

> [!NOTE]
> Pokud jste obeznámeni s architekturou ASP.NET MVC, směrování rozhraní Web API je velmi podobný směrování MVC. Hlavní rozdíl je, že webové rozhraní API používá metoda HTTP není cesta k identifikátoru URI, můžete vybrat akci. Můžete také použít MVC – vizuální styl směrování v rozhraní Web API. Tento článek nepředpokládá žádnou znalost ASP.NET MVC.


## <a name="routing-tables"></a>Směrovací tabulky

V rozhraní ASP.NET Web API *řadič* je třída, která zpracovává požadavky HTTP. Veřejné metody kontroleru se nazývají *metody akce* nebo jednoduše *akce*. Žádost o přijetí rozhraní Web API přesměruje požadavek na akci.

Chcete-li zjistit, jakou akci, která se má vyvolat, systém použije *směrovací tabulky*. Šablona projektu sady Visual Studio pro webové rozhraní API vytvoří výchozí trasy:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Tato trasa je definována v souboru WebApiConfig.cs, který je umístěn v aplikaci\_spouštěcí adresář:

![](routing-in-aspnet-web-api/_static/image1.png)

Další informace o **WebApiConfig** najdete v tématu [konfigurace ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Pokud samoobslužné hostování webového rozhraní API, je nutné nastavit směrovací tabulky přímo na **HttpSelfHostConfiguration** objektu. Další informace najdete v tématu [samoobslužné hostování webového rozhraní API](../older-versions/self-host-a-web-api.md).

Obsahuje každou položku v tabulce směrování *šablonu trasy*. Výchozí šablona trasy pro webové rozhraní API je &quot;rozhraní api / {controller} / {id}&quot;. V této šabloně &quot;api&quot; je segment cesty literálu a {controller} a {id} jsou proměnné zástupný symbol.

Rozhraní Web API přijme požadavek HTTP, pokusí se vyhledat identifikátor URI pro jeden z šablon trasy do směrovací tabulky. Pokud neodpovídá žádná trasa klient obdrží chybu 404. Například následující identifikátory URI odpovídaly výchozí trasy:

- / api/contacts
- /API/Contacts/1
- /API/Products/gizmo1

Ale následující identifikátor URI neodpovídá, protože postrádá &quot;api&quot; segmentu:

- / contacts/1

> [!NOTE]
> Pro zabránění kolizím s architekturou ASP.NET MVC směrovací je důvod pomocí "rozhraní api" v této trase. Tímto způsobem může mít &quot;/kontaktuje&quot; přejít na kontroler MVC, a &quot;/api/contacts&quot; přejít na kontroler Web API. Samozřejmě pokud se vám tato konvence, můžete změnit výchozí směrovací tabulka.

Po nalezení odpovídající trasy rozhraní Web API vybere kontroleru a akce:

- Najít kontroler webového rozhraní API přidá &quot;řadič&quot; na hodnotu *{controller}* proměnné.
- Najít akce, webové rozhraní API zjistí metodu HTTP a pak hledá akci, jejichž název začíná s tímto názvem metody HTTP. Například požadavek GET, webové rozhraní API hledá akci, která začíná &quot;získat... &quot;, jako například &quot;GetContact&quot; nebo &quot;GetAllContacts&quot;. Tato konvence platí pouze pro GET, POST, PUT a DELETE metody. Pomocí atributů na vašem řadiči můžete povolit další metody HTTP. Ukážeme příklad, který později.
- Další proměnné zástupný symbol v šabloně trasy, jako například *{id}* jsou mapovány na parametry akce.

Pojďme se podívat na příklad. Předpokládejme, že můžete definovat následující kontroler:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Tady jsou některé možné požadavků protokolu HTTP, spolu s akci, která získá pro každé vyvolání:

| Metoda HTTP | Cesta identifikátoru URI | Akce | Parametr |
| --- | --- | --- | --- |
| ZÍSKAT | rozhraní API a produktů | GetAllProducts | *(žádné)* |
| ZÍSKAT | rozhraní API, produkty nebo 4 | GetProductById | 4 |
| DELETE | rozhraní API, produkty nebo 4 | DeleteProduct | 4 |
| PŘÍSPĚVEK | rozhraní API a produktů | *(žádná shoda)* |  |

Všimněte si, že *{id}* segment identifikátoru URI, pokud jsou k dispozici, je namapována na *id* parametru akce. V tomto příkladu kontroleru definuje dvě metody GET, jednu s *id* parametr a jedna bez parametrů.

Všimněte si také, požadavek POST se nezdaří, protože kontroler nedefinuje &quot;příspěvek... &quot; metody.

## <a name="routing-variations"></a>Směrování odchylky

Předchozí část popisuje základní mechanismus směrování pro ASP.NET Web API. Tato část popisuje několik variant.

### <a name="http-methods"></a>Metody HTTP

Namísto použití zásady vytváření názvů pro metody HTTP, můžete explicitně určit metoda HTTP pro akci pomocí upravení metodu akce pomocí **HttpGet**, **HttpPut**, **HttpPost** , nebo **HttpDelete** atribut.

V následujícím příkladu je metoda FindProduct namapována na požadavky GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Povolit více metod HTTP pro akci, nebo povolíte metod HTTP než GET, PUT, POST a DELETE, použijte **AcceptVerbs** atribut, který přebírá seznam metod HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Směrování tím, že název akce

Výchozí šablonou směrování webového rozhraní API používá metodu HTTP a vyberte akci. Můžete ale také vytvořit trasu, kde je název akce součástí identifikátoru URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

V této šabloně trasy *{action}* názvy parametrů metody akce v kontroleru. S tímto stylem směrování použijte atributy k určení povolené metody HTTP. Předpokládejme například, že zařízení má následující metodu:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

V takovém případě by podrobnosti metody mapování požadavek GET pro "rozhraní api/produkty/podrobnosti/1". Tento styl směrování se podobá do architektury ASP.NET MVC a může být vhodné pro rozhraní API stylu RPC.

Název akce lze přepsat pomocí **ActionName** atribut. V následujícím příkladu jsou dvě akce, které mapují na &quot;rozhraní api, produkty/Miniatura/*id*. Jedna podporuje GET a druhý příspěvek:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Bez akce

Chcete-li metoda zabránit v získání vyvolán jako akci, použijte **NonAction** atribut. Signál, rozhraní Framework, že metoda není akci, i v případě, že odpovídají jinak pravidel směrování.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Další čtení

Toto téma poskytuje souhrnný přehled směrování. Další podrobnosti najdete v části [směrování a výběr akce](routing-and-action-selection.md), která popisuje přesně jak rozhraní odpovídá identifikátor URI pro trasu, vybere kontroler a pak vybere akce, která se má vyvolat.
