---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: "BSON podpora v rozhraní ASP.NET Web API 2.1 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
<a name="bson-support-in-aspnet-web-api-21"></a>BSON podpora v rozhraní ASP.NET Web API 2.1
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Webové rozhraní API 2.1 zavádí podporu pro BSON. Toto téma ukazuje, jak použít BSON do kontroleru webového rozhraní API (na straně serveru) a v klientské aplikace .NET.

## <a name="what-is-bson"></a>Co je BSON?

[BSON](http://bsonspec.org/) je formát binární serializace. "BSON" znamená "Binární JSON", ale jsou velmi jinak serializovat formátů BSON a JSON. Je BSON "JSON jako", protože objekty jsou reprezentovány jako dvojice název hodnota, podobně jako JSON. Na rozdíl od formát JSON číselné datové typy jsou uložena bajtů, není řetězce

BSON byla navržená tak, aby byly lightweight, proletět a rychlé kódování nebo dekódování.

- BSON je srovnatelná velikost do formátu JSON. V závislosti na data může být datovou část BSON menší nebo větší než datové části JSON. Pro serializaci binárních dat, jako je soubor bitové kopie je menší než formát JSON, BSON, protože binární data není kódováním base64.
- BSON dokumenty lze snadno kontrolu, protože elementy mají předponu pole Délka, takže analyzátor, můžete přeskočit elementy bez dekódování je.
- Kódování a dekódování jsou efektivní, protože číselné datové typy jsou uloženy jako čísla, řetězce není.

Nativní klienty, například klientské aplikace .NET, můžete využívat BSON místo založený na textu formátů například JSON a XML. Pro klienty prohlížeče budete pravděpodobně chtít přilepit s formát JSON, protože JavaScript můžete přímo převést datové části JSON.

Naštěstí webového rozhraní API používá [vyjednávání obsahu](content-negotiation.md), takže vaše rozhraní API podporuje oba formáty a zvolte klienta.

## <a name="enabling-bson-on-the-server"></a>Povolení BSON na serveru

V konfiguraci vašeho webového rozhraní API, přidejte **BsonMediaTypeFormatter** do kolekce formátování.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Pokud klient požaduje "application/bson", teď webového rozhraní API použije formátování BSON.

Chcete-li BSON přidružit jiné typy médií, přidejte ho do kolekci SupportedMediaTypes. Následující kód přidá "application/vnd.contoso" typech podporovaných médií:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Příklad relaci HTTP

V tomto příkladu použijeme následující třídy modelu plus jednoduché kontroleru webového rozhraní API:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Klient může odesílat následující požadavek HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Tady je odpovědi:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Zde I jste nahradit binární data s &quot;.&quot; znaků. Následující snímek obrazovky z Fiddler ukazuje nezpracované šestnáctkové hodnoty.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Pomocí BSON HttpClient

Klienti aplikace .NET můžete použít formátovací modul BSON s **HttpClient**. Další informace o **HttpClient**, najdete v části [volání webového rozhraní API z klienta .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Následující kód odešle požadavek GET, který přijímá BSON a pak deserializuje datovou část BSON v odpovědi.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Chcete-li požadavek BSON ze serveru, nastavte hlavičku Accept pro "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

K deserializaci text odpovědi, použijte **BsonMediaTypeFormatter**. Tento formátovací modul není v kolekci výchozí formátování, takže budete muset zadat při čtení textu odpovědi na:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Další příklad ukazuje, jak odeslat požadavek POST, který obsahuje BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Velká část tento kód je stejný jako předchozí příklad. Ale v **PostAsync** metoda, zadejte **BsonMediaTypeFormatter** jako formátovací modul:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializace nejvyšší úrovně primitivní typy

Každý dokument BSON je seznam párů klíč/hodnota. Specifikace BSON nedefinuje syntaxe pro serializaci jednu nezpracovanou hodnotu, jako je například celé číslo nebo řetězec.

Chcete-li toto omezení obejít **BsonMediaTypeFormatter** považuje za zvláštní případ primitivní typy. Před serializaci, se převede hodnotu na dvojici klíč/hodnota s klíčem "Value". Předpokládejme například, že řadiči rozhraní API vrátí celé číslo:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Před serializaci, převede formátování BSON to na následující dvojice klíč/hodnota:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Při deserializaci, převede formátování data zpět na původní hodnotu. Však klienti, kteří používají jiný analyzátor BSON muset zpracovávat tento případ, pokud webového rozhraní API vrátí nezpracované hodnoty. Obecně byste měli zvážit, vrácení strukturovaných dat, nikoli nezpracované hodnoty.

## <a name="additional-resources"></a>Další prostředky

[Ukázkové webové rozhraní API BSON](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Formátovací moduly pro média](media-formatters.md)
