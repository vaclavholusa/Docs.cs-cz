---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: V rozhraní ASP.NET Web API vyjednávání obsahu | Dokumentace Microsoftu
author: MikeWasson
description: Popisuje, jak rozhraní ASP.NET Web API implementuje vyjednávání obsahu HTTP.
ms.author: riande
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: e936bdfa52f786ec86d3e84eac3cd644225b6f92
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755206"
---
<a name="content-negotiation-in-aspnet-web-api"></a>Vyjednávání obsahu v rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Tento článek popisuje, jak rozhraní ASP.NET Web API implementuje vyjednávání obsahu.

Specifikace protokolu HTTP (RFC 2616) definuje vyjednávání obsahu jako "proces výběru nejlepší reprezentaci pro danou odpověď, když jsou k dispozici více reprezentací." Hlavní mechanismus pro vyjednávání obsahu protokolu HTTP jsou tyto hlavičky požadavku:

- **Přijměte:** typy médií, které jsou přijatelné pro odpovědi, například "application/json", "application/xml" nebo vlastního typu, jako &quot;application/vnd.example+xml&quot;
- **Přijměte-Charset:** které znakové sady jsou přijatelné, například UTF-8 nebo ISO 8859-1.
- **Přijmout kódování:** které kódování obsahu jsou přijatelné, například gzip.
- **Přijmout jazyka:** upřednostňované přirozeného jazyka, jako například "en-nám".

Server můžete se také podívat na další části požadavku HTTP. Například pokud požadavek obsahuje hlavičku X-Requested-With, označující požadavek AJAX na serveru může být jako výchozí JSON Pokud neexistuje žádné hlavičky Accept.

V tomto článku se podíváme na použití hlavičky Accept a Accept-Charset webového rozhraní API. (V tuto chvíli není předdefinovanou podporu Accept-Encoding nebo Accept-Language.)

## <a name="serialization"></a>Serializace

Pokud kontroler webového rozhraní API vrátí prostředků jako typ CLR, kanál serializuje návratovou hodnotu a zapisuje je do datové části odpovědi HTTP.

Představte si třeba následující akce kontroleru:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Klient může odeslat požadavek protokolu HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

V odpovědi může server odeslat:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

V tomto příkladu klient vyžádal JSON, Javascript nebo "nic" (\*/\*). Server odpověď s JSON s reprezentací provedených `Product` objektu. Všimněte si, že hlavička Content-Type v odpovědi nastavena na &quot;application/json&quot;.

Kontroler může také vrátit **objekt HttpResponseMessage** objektu. Pokud chcete nastavit objektu CLR pro tělo odpovědi, zavolejte **CreateResponse** – metoda rozšíření:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Tato možnost nabízí větší kontrolu nad podrobnosti odpovědi. Stavový kód, můžete nastavit přidání hlavičky protokolu HTTP a tak dále.

Objekt, který serializuje prostředek, se nazývá *formátovací modul médií*. Formátovací moduly médií odvozovat **objekt MediaTypeFormatter** třídy. Webové rozhraní API poskytuje formátovací moduly médií pro XML a JSON, a můžete vytvořit vlastní formátovací moduly pro podporu jiných typů médií. Informace o tvorbě vlastní formátování najdete v tématu [formátovací moduly médií](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Jak obsahu vyjednávání funguje

Nejprve kanál získá **IContentNegotiator** služba **HttpConfiguration** objektu. Také získá formátovací moduly médií ze seznamu **HttpConfiguration.Formatters** kolekce.

Dále volá kanál **IContentNegotiatior.Negotiate**a předejte:

- Typ objektu určeného k serializaci
- Kolekce formátovací moduly médií
- Požadavek HTTP

**Negotiate** metoda vrací dva druhy údajů:

- Formátování, které použito
- Typ média pro odpověď

Pokud není formátování nalezeno, **Negotiate** vrátí metoda **null**a chyba klienta recevies HTTP 406 (nepřijatelné).

Následující kód ukazuje, jak můžete kontroler přímo vyvolat vyjednávání obsahu:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Tento kód je ekvivalentní k co kanál provádí automaticky.

## <a name="default-content-negotiator"></a>Výchozí Negotiator obsahu

**DefaultContentNegotiator** třída poskytuje výchozí implementaci třídy **IContentNegotiator**. Vyberte formátovací modul používá několik kritérií.

Formátovací modul, nejprve musí být schopen daný typ serializovat. To je ověřený pomocí volání **MediaTypeFormatter.CanWriteType**.

Vyjednavač obsahu dále zjistí jednotlivé formátovací moduly a vyhodnotí, jak dobře odpovídá požadavku HTTP. K vyhodnocení shody, vyjednavače obsahu prohledá dvě věci ve formátování:

- **SupportedMediaTypes** kolekce, která obsahuje seznam podporovanými typy médií. Vyjednavač obsahu se pokusí shodovat se tento seznam u této žádosti hlavičky Accept. Všimněte si, že může obsahovat hlavičku Accept rozsahy. Například "text/plain" je nalezena shoda s text /\* nebo \* / \*.
- **MediaTypeMappings** kolekce, která obsahuje seznam **MediaTypeMapping** objekty. **MediaTypeMapping** třída poskytuje obecný způsob, jak porovnávají požadavky HTTP s typy médií. Je třeba namapovat vlastní hlavičku HTTP pro určitý typ média.

Pokud existuje více odpovídá, shoda s nejvyšší kvality faktor wins. Příklad:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

V tomto příkladu má application/json faktor implicitní kvality 1.0, takže je upřednostňované nad application/xml.

Pokud nejsou nalezeny žádné shody, vyjednavače obsahu pokusí porovnávaný typ média obsahu žádosti případné. Například pokud požadavek obsahuje JSON data, vyjednavače obsahu vypadá formátování JSON.

Pokud ještě neexistují žádné odpovídající položky, vyjednavače obsahu jednoduše vybere první formátovací modul, který dokáže daný typ serializovat.

## <a name="selecting-a-character-encoding"></a>Výběr kódování znaků

Po výběru formátovacího modulu vyjednavače obsahu zvolí nejvhodnější kódování znaků pohledem **SupportedEncodings** vlastnosti formátování a shod pro hlavičku Accept-Charset v požadavku (pokud existuje).
