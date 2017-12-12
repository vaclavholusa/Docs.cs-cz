---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: "Formátovací moduly pro média v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7d85b995cd577d0ff90fe96bce508c7fbdc6ebbb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Formátovací moduly pro média v rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Tento kurz ukazuje jak podporují formátů médií s další v rozhraní ASP.NET Web API.

## <a name="internet-media-types"></a>Typy médií Internetu

Typ média, označované taky jako typ MIME, identifikuje formát část data. V protokolu HTTP popisují typy médií formátu textu zprávy. Typ média se skládá z dva řetězce, typ a podtyp. Příklad:

- text/html
- bitové kopie nebo png
- Application/json

Když zprávy HTTP obsahuje obsah entity, určuje hlavičku Content-Type formát textu zprávy. Tato hodnota informuje příjemce jak analyzovat obsah textu zprávy.

Například pokud odpovědi HTTP obsahuje bitovou kopii PNG, odpověď může mít následující hlavičky.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Když klient odešle zprávu požadavku, může zahrnovat hlavičky Accept. Hlavička Accept informuje, že server, které média typu (typů) klienta chce ze serveru. Příklad:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Tuto hlavičku informuje server, že klienta chce HTML, XHTML nebo XML.

Typ média určuje, jak webové rozhraní API serializuje a deserializuje obsah zprávy HTTP. Webové rozhraní API má integrovanou podporu pro XML, JSON, BSON a data formuláře urlencoded a může podporovat dalšího média typy zápis *formátovací modul média*.

Pokud chcete vytvořit formátovací modul médií, odvozena od jedné z těchto tříd:

- [Objekt MediaTypeFormatter](https://msdn.microsoft.com/en-us/library/system.net.http.formatting.mediatypeformatter.aspx). Tato třída používá asynchronního čtení a zápisu metody.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/en-us/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Tato třída odvozená z **objekt MediaTypeFormatter** ale používá sychronous metody pro čtení a zápis.

Odvozování z **BufferedMediaTypeFormatter** je jednodušší, protože neexistuje žádný asynchronní kód, ale také znamená to během vstupně-výstupních operací můžete blokovat volající vlákno.

## <a name="example-creating-a-csv-media-formatter"></a>Příklad: Vytvoření média formátování sdíleného svazku clusteru

Následující příklad ukazuje formátovací modul typu média, která může serializovat objekt produktu do formátu hodnot oddělených čárkami (CSV). Tento příklad používá typ produktu definované v tomto kurzu [vytváření webového rozhraní API této operace CRUD podporuje](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Zde je definice objektu produktu:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

K implementaci formátování CSV, definice třídy, která je odvozena z **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

V konstruktoru přidejte typy médií podporovanými formátování podporuje. V tomto příkladu formátování podporuje typu jedno médium, &quot;text/csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Přepsání **CanWriteType** může serializovat metoda k označení, které typy formátování:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

V tomto příkladu může serializovat jedné formátovací modul `Product` objekty a také kolekce `Product` objekty.

Podobně přepsat **CanReadType** metoda k označení, které typy formátovací modul může deserializovat. V tomto příkladu formátování nepodporuje deserializace, takže jednoduše vrátí metoda **false**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Nakonec přepsání **WriteToStream** metoda. Tato metoda serializuje typu pomocí zápisu do datového proudu. Pokud vaše formátovací modul podporuje deserializace, také přepsat **ReadFromStream** metoda.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Přidání média formátování do kanálu webové rozhraní API

Přidat typy médií formátovacího modulu do kanálu webové rozhraní API, použijte **formátování** vlastnost **HttpConfiguration** objektu.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Kódování znaků

Formátovací modul média může volitelně podporovat více kódování znaků, jako je například UTF-8 nebo ISO 8859-1.

V konstruktoru, přidejte jeden nebo více [System.Text.Encoding](https://msdn.microsoft.com/en-us/library/system.text.encoding.aspx) typů, který **SupportedEncodings** kolekce. Uveďte výchozí kódování první.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

V **WriteToStream** a **ReadFromStream** volání metody, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/en-us/library/hh969054.aspx) vyberte kódování upřednostňované znaků. Tato metoda odpovídá hlavičky žádosti podle seznamu podporovaných kódování. Pomocí vráceného **kódování** při čtení nebo zápisu do datového proudu:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
