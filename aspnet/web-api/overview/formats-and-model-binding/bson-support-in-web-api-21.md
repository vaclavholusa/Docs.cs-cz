---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Podpora formátu BSON ve webovém rozhraní API 2.1 technologie ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7b9a0c2def4703f6fad65790dfe4227117189933
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839875"
---
<a name="bson-support-in-aspnet-web-api-21"></a>Podpora formátu BSON ve webovém rozhraní API 2.1 technologie ASP.NET
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Webové rozhraní API 2.1 přináší podporu pro BSON. Toto téma ukazuje, jak použít BSON ve vaší kontroler Web API (na straně serveru) a v klientské aplikaci .NET.

## <a name="what-is-bson"></a>Co je BSON?

[BSON](http://bsonspec.org/) je binární serializační formát. "BSON" znamená "Binární JSON", ale formátů BSON a JSON serializují nějak významně odlišně. BSON je "Podobných formátu JSON", protože objekty jsou reprezentovány ve formě dvojice název hodnota, podobně jako JSON. Na rozdíl od JSON číselné datové typy jsou uloženy jako bajtů, ne řetězce

BSON je navržený tak, že byly zjednodušené, rychle proletět a rychlé kódování/dekódování.

- BSON je srovnatelné velikosti do formátu JSON. V závislosti na data může být datová část BSON menší nebo větší než datovou část JSON. Pro serializaci binárních dat, jako je například soubor obrázku je BSON menší než JSON, protože není binární data s kódováním base64.
- BSON dokumenty jsou usnadňuje kontrolu, protože elementy mají předponu pole length, tak analyzátor mohli přejít rovnou elementy bez dekódování je.
- Kódování a dekódování jsou efektivní, protože číselné datové typy jsou uloženy jako čísla, řetězce není.

Nativní klienty, jako je například klientské aplikace .NET, můžete s výhodou využít BSON místo textové formáty, jako je JSON nebo XML. Pro klienty prohlížeče bude pravděpodobně chcete zůstat u JSON, protože JavaScript můžete přímo převést datovou část JSON.

Naštěstí webového rozhraní API používá [vyjednávání obsahu](content-negotiation.md), takže můžete podporují oba formáty a umožnit klienta, vyberte rozhraní API.

## <a name="enabling-bson-on-the-server"></a>Povolení BSON na serveru

V konfiguraci webového rozhraní API, přidejte **BsonMediaTypeFormatter** do kolekce formátovacích modulů.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Pokud si klient vyžádá "application/bson", teď webové rozhraní API použije BSON formátovací modul.

BSON přidružit jiné typy médií, přidejte je do kolekce SupportedMediaTypes. Následující kód přidá do typů médií podporovaných "application/vnd.contoso":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Příklad relace HTTP

V tomto příkladu použijeme následující třídy modelu a jednoduché kontroler Web API:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Klient může odesílat následující požadavek protokolu HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Tady je odpovědi:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Tady mám jsme nahradit binární data s využitím &quot;.&quot; znaků. Následující snímek obrazovky z Fiddleru zobrazí nezpracované šestnáctkové hodnoty.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>BSON pomocí HttpClient

Klienti aplikací .NET můžete použít formátování BSON s **HttpClient**. Další informace o **HttpClient**, naleznete v tématu [volání webového rozhraní API z klienta .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Následující kód odešle požadavek GET, která přijímá BSON a potom deserializuje datovou část BSON v odpovědi.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Chcete-li BSON vyžádat ze serveru, nastavte hlavičku Accept pro "application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Chcete-li deserializovat tělo odpovědi, použijte **BsonMediaTypeFormatter**. Tento formátovací modul není v kolekci výchozí formátování, takže budete muset zadat při čtení textu odpovědi:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Následující příklad ukazuje, jak odeslat požadavek POST, který obsahuje BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Velká část tento kód je stejný jako předchozí příklad. Ale **PostAsync** metody zadejte **BsonMediaTypeFormatter** jako formátovací modul:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializace nejvyšší úrovně primitivní typy

Každý dokument BSON je seznam dvojic klíč/hodnota. Specifikace formátu BSON nedefinuje syntaxe pro serializaci nezpracované hodnotu single, jako je například celé číslo nebo řetězec.

Chcete-li toto omezení **BsonMediaTypeFormatter** považuje za zvláštní případ primitivní typy. Před serializací, převede hodnotu na dvojici klíč/hodnota s klíčem "Value". Například předpokládejme, že vaše kontroleru rozhraní API vrátí celé číslo:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Před serializací, formátovací modul BSON převede na následující dvojici klíč/hodnota:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Při deserializaci, formátovací modul převádí data zpět na původní hodnotu. Však klienti, kteří používají jiný analyzátor BSON potřeba zpracovávat tento případ, pokud vaše webové rozhraní API vrátí nezpracované hodnoty. Obecně platí měli byste zvážit, strukturovaná data, nikoli nezpracované hodnoty vrácením.

## <a name="additional-resources"></a>Další prostředky

[Ukázkové webové rozhraní API BSON](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Formátovací moduly médií](media-formatters.md)
