---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Směrování v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26567082"
---
<a name="routing-in-aspnet-web-api"></a>Směrování v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Tento článek popisuje, jak rozhraní ASP.NET Web API směruje požadavky HTTP na řadiče.

> [!NOTE]
> Pokud jste obeznámeni s architekturou ASP.NET MVC, směrování rozhraní Web API je velmi podobný směrování MVC. Hlavní rozdíl je, že webového rozhraní API používá metoda HTTP není cesta k identifikátoru URI, můžete vybrat akci. Můžete také použít MVC stylu směrování v rozhraní Web API. Tento článek nepřebírá žádnou znalost rozhraní ASP.NET MVC.


## <a name="routing-tables"></a>Směrovacích tabulek

V rozhraní ASP.NET Web API *řadič* je třída, která zpracovává požadavky HTTP. Veřejné metody řadiče se nazývají *metody akce* nebo jednoduše *akce*. Když rozhraní Web API přijme požadavek, přesměruje požadavek na akci.

Určit akci, která má být vyvolán, používá rozhraní *směrovací tabulky*. Šablona projektu sady Visual Studio pro webové rozhraní API vytvoří výchozí trasa:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Tato trasa je definována v souboru WebApiConfig.cs, který je umístěn v aplikaci\_spouštěcí adresář:

![](routing-in-aspnet-web-api/_static/image1.png)

Další informace o **WebApiConfig** třídy najdete v tématu [konfigurace rozhraní ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Pokud hostujete samoobslužné webové rozhraní API, musíte nastavit do směrovací tabulky přímo na **HttpSelfHostConfiguration** objektu. Další informace najdete v tématu [hostování na vlastním serveru webového rozhraní API](../older-versions/self-host-a-web-api.md).

Obsahuje každou položku v tabulce směrování *šablonu trasy*. Výchozí šablona trasy pro webového rozhraní API je &quot;rozhraní api nebo {controller} / {id}&quot;. V této šabloně &quot;rozhraní api&quot; je segment literálu cesty a {controller} a {id} jsou proměnné zástupný symbol.

Když rozhraní Web API obdrží požadavek HTTP, pokouší se identifikátor URI pro jeden z šablon trasy do směrovací tabulky. Pokud žádná trasa odpovídá, klient obdrží chybu 404. Například následující identifikátory URI odpovídají výchozí trasu:

- / api/contacts
- /API/Contacts/1
- /API/Products/gizmo1

Ale následující identifikátor URI neodpovídá, protože jí chybí &quot;rozhraní api&quot; segmentu:

- / contacts/1

> [!NOTE]
> Důvod pomocí "rozhraní api" v postupu je zabrání se tím kolizím s architekturou ASP.NET MVC směrování. Tímto způsobem může mít &quot;/kontaktuje&quot; přejděte na řadič MVC a &quot;/api/contacts&quot; přejděte do kontroleru webového rozhraní API. Samozřejmě pokud vám nevyhovuje touto konvencí, můžete změnit výchozí směrovací tabulka.

Jakmile je nalezen odpovídající trasy, vybere webového rozhraní API kontroleru a akce:

- Najít kontroler, přidá webového rozhraní API &quot;řadič&quot; na hodnotu *{controller}* proměnné.
- Akce najdete webového rozhraní API porovná metodu protokolu HTTP a pak hledá akce jejichž název začíná tímto názvem metoda HTTP. Například s požadavek GET webového rozhraní API vypadá pro akci, která začíná &quot;získat... &quot;, jako například &quot;GetContact&quot; nebo &quot;GetAllContacts&quot;. Touto konvencí platí pouze pro GET, POST, PUT a DELETE metody. Můžete povolit další metody HTTP pomocí atributů na vašem řadiči. Příklad této uvidíme později.
- Ostatní proměnné zástupný symbol v šabloně trasy, jako *{id}* jsou namapované na parametrů akcí.

Podívejme se na příklad. Předpokládejme, že definovat následující řadiče:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Zde jsou některé možné požadavků protokolu HTTP, společně s akci, která volán pro každou:

| Metoda HTTP | Cesta URI | Akce | Parametr |
| --- | --- | --- | --- |
| GET | rozhraní API nebo produkty | GetAllProducts | *(žádný)* |
| GET | rozhraní API, produkty nebo 4 | GetProductById | 4 |
| DELETE | rozhraní API, produkty nebo 4 | DeleteProduct | 4 |
| POST | rozhraní API nebo produkty | *(žádná shoda)* |  |

Všimněte si, že *{id}* segment identifikátoru URI, pokud existuje, je namapovaný na *id* parametr akce. V tomto příkladu řadičem definuje dvě metody GET, jeden s *id* parametr a jeden bez parametrů.

Také Upozorňujeme, že požadavek POST se nezdaří, protože nedefinuje kontroleru &quot;Post... &quot; metoda.

## <a name="routing-variations"></a>Směrování variant

Popsané v předchozí části základní mechanismus směrování pro ASP.NET Web API. Tato část popisuje některé rozdíly.

### <a name="http-methods"></a>Metody HTTP

Místo použití zásady vytváření názvů pro metody HTTP, můžete explicitně zadat metoda HTTP pro akce architekturu metoda akce s **třídy MetadataExchangeClientMode**, **HttpPut**, **HttpPost** , nebo **HttpDelete** atribut.

V následujícím příkladu je metoda FindProduct namapovaný na požadavky GET:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Povolit více metod HTTP pro akci, nebo povolíte metod HTTP než GET, PUT, POST a odstranění, použijte **AcceptVerbs** atribut, který přebírá seznam metod HTTP.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Směrování podle názvu akce

Výchozí směrování šablonu webového rozhraní API používá metodu protokolu HTTP a vyberte akci. Můžete však také vytvořit trasu, kde je název akce zahrnutá v identifikátoru URI:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

V této šabloně trasy *{action}* názvů parametrů metody akce v kontroleru. Pomocí této styl směrování slouží k zadání povolených metod HTTP atributy. Předpokládejme například, že herní zařízení má následující metodu:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

V takovém případě by metodu podrobnosti mapování požadavek GET pro "api/produkty/podrobnosti/1". Tento styl směrování je podobná ASP.NET MVC a může být vhodné pro rozhraní API stylu RPC.

Název akce můžete přepsat pomocí **název akce** atribut. V následujícím příkladu, existují dvě akce, které jsou mapovány na &quot;rozhraní api, produkty nebo miniaturu/*id*. Jedna podporuje GET a dalších POST:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Jiné akce

Abyste zabránili získávání vyvolána jako akce metodu, pomocí **NonAction** atribut. To signalizuje do rozhraní metoda není akce, i v případě, že by se jinak neshodovaly pravidla směrování.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Další čtení

Toto téma poskytuje podrobný pohled směrování. Další podrobnosti naleznete v [směrování a výběr akce](routing-and-action-selection.md), který popisuje, přesně jak rozhraní odpovídá identifikátor URI pro trasu, vybere řadič a potom vybere akci k vyvolání.
