---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Formátovací moduly médií ve rozhraní ASP.NET Web API 2 | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 7b7ba2fb3f1bba0447e700c84a017266cba305e6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757111"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="05a1c-102">Formátovací moduly médií ve rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="05a1c-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="05a1c-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="05a1c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="05a1c-104">Tento kurz ukazuje, jak Podpora formátů dalšího média v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="05a1c-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="05a1c-105">Typy médií Internet</span><span class="sxs-lookup"><span data-stu-id="05a1c-105">Internet Media Types</span></span>

<span data-ttu-id="05a1c-106">Typ média, taky jako typ MIME, identifikuje formát část data.</span><span class="sxs-lookup"><span data-stu-id="05a1c-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="05a1c-107">V protokolu HTTP popisují typy médií formátu těla zprávy.</span><span class="sxs-lookup"><span data-stu-id="05a1c-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="05a1c-108">Typ média se skládá z dvou řetězců, typ a podtyp.</span><span class="sxs-lookup"><span data-stu-id="05a1c-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="05a1c-109">Příklad:</span><span class="sxs-lookup"><span data-stu-id="05a1c-109">For example:</span></span>

- <span data-ttu-id="05a1c-110">text/html</span><span class="sxs-lookup"><span data-stu-id="05a1c-110">text/html</span></span>
- <span data-ttu-id="05a1c-111">Image/png</span><span class="sxs-lookup"><span data-stu-id="05a1c-111">image/png</span></span>
- <span data-ttu-id="05a1c-112">Application/json</span><span class="sxs-lookup"><span data-stu-id="05a1c-112">application/json</span></span>

<span data-ttu-id="05a1c-113">Pokud zpráva HTTP obsahuje obsah entity, Hlavička Content-Type určuje formát těla zprávy.</span><span class="sxs-lookup"><span data-stu-id="05a1c-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="05a1c-114">To informuje příjemce, jak analyzovat obsah textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="05a1c-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="05a1c-115">Například pokud odpověď HTTP obsahuje obrázek PNG, odpověď může obsahovat tato záhlaví.</span><span class="sxs-lookup"><span data-stu-id="05a1c-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="05a1c-116">Když klient odešle zprávu požadavku, může obsahovat hlavičku Accept.</span><span class="sxs-lookup"><span data-stu-id="05a1c-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="05a1c-117">Hlavička Accept říká, že server, klient typy médií, které chce ze serveru.</span><span class="sxs-lookup"><span data-stu-id="05a1c-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="05a1c-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="05a1c-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="05a1c-119">Toto záhlaví říká serveru, vyžaduje klient HTML, XHTML a XML.</span><span class="sxs-lookup"><span data-stu-id="05a1c-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="05a1c-120">Typ média určuje, jak webové rozhraní API serializuje a deserializuje tělo zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="05a1c-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="05a1c-121">Webové rozhraní API obsahuje integrovanou podporu XML, JSON, BSON a data formuláře kódovaná a může podporovat další média typy zápisem *formátovací modul médií*.</span><span class="sxs-lookup"><span data-stu-id="05a1c-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="05a1c-122">Pokud chcete vytvořit formátovací modul médií, odvození od některého z těchto tříd:</span><span class="sxs-lookup"><span data-stu-id="05a1c-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="05a1c-123">[Objekt MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="05a1c-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="05a1c-124">Tato třída používá asynchronní čtení a zápis metody.</span><span class="sxs-lookup"><span data-stu-id="05a1c-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="05a1c-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="05a1c-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="05a1c-126">Tato třída je odvozena z **objekt MediaTypeFormatter** , ale používá sychronous metody pro čtení a zápisu.</span><span class="sxs-lookup"><span data-stu-id="05a1c-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="05a1c-127">Odvozování z **BufferedMediaTypeFormatter** je jednodušší, protože neexistuje žádný asynchronní kód, ale také to znamená volající vlákno může přerušováno během vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="05a1c-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="05a1c-128">Příklad: Vytvoření médií formátovacího modulu sdíleného svazku clusteru</span><span class="sxs-lookup"><span data-stu-id="05a1c-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="05a1c-129">Následující příklad ukazuje formátovací modul typu média, která může serializovat objekt produktu do formátu hodnot oddělených čárkami (CSV).</span><span class="sxs-lookup"><span data-stu-id="05a1c-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="05a1c-130">Tento příklad používá produkt typ definovaný v tomto kurzu [této operace CRUD podporuje vytvoření webového rozhraní API](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="05a1c-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="05a1c-131">Tady je definice objektu produktu:</span><span class="sxs-lookup"><span data-stu-id="05a1c-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="05a1c-132">Chcete-li implementovat formátovací modul sdíleného svazku clusteru, definovat třídu, která je odvozena z **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="05a1c-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="05a1c-133">V konstruktoru přidejte typy médií podporovanými formátovacím podporuje.</span><span class="sxs-lookup"><span data-stu-id="05a1c-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="05a1c-134">V tomto příkladu podporuje formátovací modul typu média jeden &quot;text/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="05a1c-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="05a1c-135">Přepsat **CanWriteType** indikace toho, jakého typu formátování může serializovat:</span><span class="sxs-lookup"><span data-stu-id="05a1c-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="05a1c-136">V tomto příkladu může serializovat formátování jednoho `Product` objekty a kolekce `Product` objekty.</span><span class="sxs-lookup"><span data-stu-id="05a1c-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="05a1c-137">Podobně, přepsat **CanReadType** indikace toho, jakého typu formátování může deserializovat.</span><span class="sxs-lookup"><span data-stu-id="05a1c-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="05a1c-138">V tomto příkladu formátovací modul nepodporuje deserializace, takže metoda jednoduše vrací **false**.</span><span class="sxs-lookup"><span data-stu-id="05a1c-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="05a1c-139">Nakonec přepsání **WriteToStream** metody.</span><span class="sxs-lookup"><span data-stu-id="05a1c-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="05a1c-140">Tato metoda serializuje typu pomocí zápisu do datového proudu.</span><span class="sxs-lookup"><span data-stu-id="05a1c-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="05a1c-141">Pokud vaše formátovací modul podporuje deserializace, také přepsat **ReadFromStream** metody.</span><span class="sxs-lookup"><span data-stu-id="05a1c-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="05a1c-142">Přidání formátovací modul médií do kanálu webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="05a1c-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="05a1c-143">Chcete-li přidat typy médií formátovacího modulu do kanálu webové rozhraní API, použijte **formátovací moduly** vlastnost **HttpConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="05a1c-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="05a1c-144">Kódování znaků</span><span class="sxs-lookup"><span data-stu-id="05a1c-144">Character Encodings</span></span>

<span data-ttu-id="05a1c-145">Formátovací modul médií může volitelně podporovat více kódování znaků, například UTF-8 nebo ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="05a1c-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="05a1c-146">V konstruktoru, přidejte jeden nebo více [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) typy mají **SupportedEncodings** kolekce.</span><span class="sxs-lookup"><span data-stu-id="05a1c-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="05a1c-147">Dejte výchozí kódování první.</span><span class="sxs-lookup"><span data-stu-id="05a1c-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="05a1c-148">V **WriteToStream** a **ReadFromStream** volání metody, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) vybrat kódování upřednostňované znak.</span><span class="sxs-lookup"><span data-stu-id="05a1c-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="05a1c-149">Tato metoda odpovídá záhlaví požadavku na seznam podporovaných kódování.</span><span class="sxs-lookup"><span data-stu-id="05a1c-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="05a1c-150">Pomocí vráceného **kódování** při čtení nebo zápisu z datového proudu:</span><span class="sxs-lookup"><span data-stu-id="05a1c-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
