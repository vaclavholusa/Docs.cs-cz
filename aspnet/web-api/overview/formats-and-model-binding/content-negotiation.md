---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: "Obsahu vyjednávání v rozhraní ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: "Popisuje, jak rozhraní ASP.NET Web API implementuje vyjednávání obsahu HTTP."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="content-negotiation-in-aspnet-web-api"></a>Vyjednávání obsahu v rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Tento článek popisuje, jak rozhraní ASP.NET Web API implementuje vyjednávání obsahu.

Specifikace protokolu HTTP (RFC 2616) definuje vyjednávání obsahu jako "proces výběru nejlepší reprezentace pro dané odpovědi, když jsou k dispozici více reprezentace." Primární mechanismus pro vyjednávání obsahu protokolu HTTP jsou tyto hlavičky požadavku:

- **Přijměte:** typů médií, které jsou přijatelné pro odpovědi, například "application/json", "application/xml" nebo typ vlastní média, jako &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** které znakové sady jsou přijatelné, jako je například UTF-8 nebo ISO 8859-1.
- **Přijměte-Encoding:** které kódování obsahu jsou přijatelné, například gzip.
- **Přijmout jazyk:** upřednostňované přirozeného jazyka, jako "en-us".

Server můžete také zobrazit další části požadavku HTTP. Například pokud požadavek obsahuje hlavičku X-Requested-With, označující požadavek AJAX serveru může výchozí JSON Pokud neexistuje žádné hlavičky Accept.

V tomto článku se podíváme používání hlavičky Accept a Accept-Charset webového rozhraní API. (V tomto okamžiku se žádné integrovanou podporu pro Accept-Encoding nebo Accept-Language.)

## <a name="serialization"></a>Serializace

Pokud kontroler Web API vrátí prostředků jako typ CLR, kanál návratovou hodnotu serializuje a zapíše do odpovědi HTTP.

Zvažte například následující akce kontroleru:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Klient může odesílat tento požadavek HTTP:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

V odpovědi může odeslat na server:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

V tomto příkladu si klient vyžádal JSON, Javascript nebo "nic" (\*/\*). Server odpověď má reprezentaci JSON `Product` objektu. Všimněte si, že v odpovědi hlavičky Content-Type je nastaven na &quot;application/json&quot;.

Můžete se taky vrátit řadič **objekt HttpResponseMessage** objektu. Chcete-li zadat objekt CLR pro text odpovědi, zavolejte **CreateResponse** metoda rozšíření:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Tato možnost vám dává větší kontrolu nad podrobnosti o odpovědi. Můžete nastavit kód stavu, přidejte hlavičky protokolu HTTP a tak dále.

Objekt, který serializuje prostředku se nazývá *formátovací modul média*. Formátovací moduly pro média odvozena od **objekt MediaTypeFormatter** třídy. Webové rozhraní API poskytuje formátovací moduly pro média pro soubor XML a JSON a můžete vytvořit vlastní formátovací moduly, které podporují jiné typy médií. Informace o vytváření vlastních formátování najdete v tématu [formátovací moduly pro média](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Jak obsahu vyjednávání funguje

Nejdřív získá kanálu **IContentNegotiator** služby z **HttpConfiguration** objektu. Také získá formátovací moduly pro média ze seznamu **HttpConfiguration.Formatters** kolekce.

V dalším kroku kanálu volání **IContentNegotiatior.Negotiate**a předejte:

- Typ k serializaci objektu
- Kolekce formátovací moduly pro média
- Požadavek HTTP

**Negotiate** metoda vrátí dva údaje:

- Které formátovací modul použít
- Typ média pro odpověď

Pokud není formátování nalezeno, **Negotiate** metoda vrátí **null**a chyba recevies HTTP klienta 406 (nepřijatelný).

Následující kód ukazuje, jak řadič přímo vyvolat vyjednávání obsahu:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Tento kód je ekvivalentní co kanálu provede automaticky.

## <a name="default-content-negotiator"></a>Vyjednavač obsahu výchozí

**DefaultContentNegotiator** třída poskytuje výchozí implementaci **IContentNegotiator**. Chcete-li vybrat formátovací modul používá několik kritéria.

Formátovací modul nejprve musí být schopen daný typ serializovat. To je ověřit voláním **MediaTypeFormatter.CanWriteType**.

Vyjednavač obsahu v dalším kroku vypadá na jednotlivé formátovací moduly a vyhodnotí, jak dobře odpovídá požadavek HTTP. K vyhodnocení shody, vypadá vyjednavač obsahu na dvě věci na formátovací modul:

- **SupportedMediaTypes** kolekce, který obsahuje seznam podporovanými typy médií. Vyjednavač obsahu se pokusí vyhledat tento seznam u této žádosti hlavička Accept. Všimněte si, že hlavičku Accept může obsahovat rozsahy. Například "text/plain" je nalezena shoda pro text /\* nebo \* / \*.
- **MediaTypeMappings** kolekce, který obsahuje seznam **MediaTypeMapping** objekty. **MediaTypeMapping** třída poskytuje obecné způsob, jak porovnávají požadavky HTTP s typy médií. Například je může namapovat vlastní hlavičku HTTP pro určitý typ média.

Pokud máte více odpovídá, shoda s nejvyšší wins koeficientu kvality. Příklad:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

V tomto příkladu application/json má faktor předpokládané kvality 1.0, takže je upřednostňovaný nad application/xml.

Pokud nejsou nalezeny žádné shody, vyjednavač obsahu pokusí porovnávaný typ média obsahu žádosti, pokud existuje. Například pokud požadavek obsahuje JSON data, vyjednavač obsahu vypadá formátování JSON.

Pokud ještě nejsou žádné shody, vyjednavač obsahu jednoduše vybere první formátovací modul, který dokáže daný typ serializovat.

## <a name="selecting-a-character-encoding"></a>Výběr kódování znaků

Po výběru formátování se vyjednavač obsahu vybere nejvhodnější kódování znaků prohlížením **SupportedEncodings** vlastnost formátování a odpovídající proti hlavičku Accept-Charset v požadavku (pokud existuje).
