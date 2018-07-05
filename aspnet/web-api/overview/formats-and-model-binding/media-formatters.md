---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formátovací moduly médií ve rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 824fea5a8837ff8b09af832b7d2a094c7d82907b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368913"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Formátovací moduly médií ve rozhraní ASP.NET Web API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Tento kurz ukazuje, jak Podpora formátů dalšího média v rozhraní ASP.NET Web API.

## <a name="internet-media-types"></a>Typy médií Internet

Typ média, taky jako typ MIME, identifikuje formát část data. V protokolu HTTP popisují typy médií formátu těla zprávy. Typ média se skládá z dvou řetězců, typ a podtyp. Příklad:

- text/html
- Image/png
- Application/json

Pokud zpráva HTTP obsahuje obsah entity, Hlavička Content-Type určuje formát těla zprávy. To informuje příjemce, jak analyzovat obsah textu zprávy.

Například pokud odpověď HTTP obsahuje obrázek PNG, odpověď může obsahovat tato záhlaví.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Když klient odešle zprávu požadavku, může obsahovat hlavičku Accept. Hlavička Accept říká, že server, klient typy médií, které chce ze serveru. Příklad:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Toto záhlaví říká serveru, vyžaduje klient HTML, XHTML a XML.

Typ média určuje, jak webové rozhraní API serializuje a deserializuje tělo zprávy HTTP. Webové rozhraní API obsahuje integrovanou podporu XML, JSON, BSON a data formuláře kódovaná a může podporovat další média typy zápisem *formátovací modul médií*.

Pokud chcete vytvořit formátovací modul médií, odvození od některého z těchto tříd:

- [Objekt MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). Tato třída používá asynchronní čtení a zápis metody.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Tato třída je odvozena z **objekt MediaTypeFormatter** , ale používá sychronous metody pro čtení a zápisu.

Odvozování z **BufferedMediaTypeFormatter** je jednodušší, protože neexistuje žádný asynchronní kód, ale také to znamená volající vlákno může přerušováno během vstupně-výstupních operací.

## <a name="example-creating-a-csv-media-formatter"></a>Příklad: Vytvoření médií formátovacího modulu sdíleného svazku clusteru

Následující příklad ukazuje formátovací modul typu média, která může serializovat objekt produktu do formátu hodnot oddělených čárkami (CSV). Tento příklad používá produkt typ definovaný v tomto kurzu [této operace CRUD podporuje vytvoření webového rozhraní API](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Tady je definice objektu produktu:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Chcete-li implementovat formátovací modul sdíleného svazku clusteru, definovat třídu, která je odvozena z **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

V konstruktoru přidejte typy médií podporovanými formátovacím podporuje. V tomto příkladu podporuje formátovací modul typu média jeden &quot;text/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Přepsat **CanWriteType** indikace toho, jakého typu formátování může serializovat:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

V tomto příkladu může serializovat formátování jednoho `Product` objekty a kolekce `Product` objekty.

Podobně, přepsat **CanReadType** indikace toho, jakého typu formátování může deserializovat. V tomto příkladu formátovací modul nepodporuje deserializace, takže metoda jednoduše vrací **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Nakonec přepsání **WriteToStream** metody. Tato metoda serializuje typu pomocí zápisu do datového proudu. Pokud vaše formátovací modul podporuje deserializace, také přepsat **ReadFromStream** metody.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Přidání formátovací modul médií do kanálu webové rozhraní API

Chcete-li přidat typy médií formátovacího modulu do kanálu webové rozhraní API, použijte **formátovací moduly** vlastnost **HttpConfiguration** objektu.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Kódování znaků

Formátovací modul médií může volitelně podporovat více kódování znaků, například UTF-8 nebo ISO 8859-1.

V konstruktoru, přidejte jeden nebo více [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) typy mají **SupportedEncodings** kolekce. Dejte výchozí kódování první.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

V **WriteToStream** a **ReadFromStream** volání metody, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) vybrat kódování upřednostňované znak. Tato metoda odpovídá záhlaví požadavku na seznam podporovaných kódování. Pomocí vráceného **kódování** při čtení nebo zápisu z datového proudu:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
