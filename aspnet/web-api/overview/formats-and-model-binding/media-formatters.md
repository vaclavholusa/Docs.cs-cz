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
ms.openlocfilehash: 9103574597df126a22e21a2f51815f608e46f47f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="75d96-102">Formátovací moduly pro média v rozhraní ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="75d96-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="75d96-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="75d96-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="75d96-104">Tento kurz ukazuje jak podporují formátů médií s další v rozhraní ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="75d96-104">This tutorial shows how support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="75d96-105">Typy médií Internetu</span><span class="sxs-lookup"><span data-stu-id="75d96-105">Internet Media Types</span></span>

<span data-ttu-id="75d96-106">Typ média, označované taky jako typ MIME, identifikuje formát část data.</span><span class="sxs-lookup"><span data-stu-id="75d96-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="75d96-107">V protokolu HTTP popisují typy médií formátu textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="75d96-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="75d96-108">Typ média se skládá z dva řetězce, typ a podtyp.</span><span class="sxs-lookup"><span data-stu-id="75d96-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="75d96-109">Příklad:</span><span class="sxs-lookup"><span data-stu-id="75d96-109">For example:</span></span>

- <span data-ttu-id="75d96-110">text/html</span><span class="sxs-lookup"><span data-stu-id="75d96-110">text/html</span></span>
- <span data-ttu-id="75d96-111">image/png</span><span class="sxs-lookup"><span data-stu-id="75d96-111">image/png</span></span>
- <span data-ttu-id="75d96-112">Application/json</span><span class="sxs-lookup"><span data-stu-id="75d96-112">application/json</span></span>

<span data-ttu-id="75d96-113">Když zprávy HTTP obsahuje obsah entity, určuje hlavičku Content-Type formát textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="75d96-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="75d96-114">Tato hodnota informuje příjemce jak analyzovat obsah textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="75d96-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="75d96-115">Například pokud odpovědi HTTP obsahuje bitovou kopii PNG, odpověď může mít následující hlavičky.</span><span class="sxs-lookup"><span data-stu-id="75d96-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="75d96-116">Když klient odešle zprávu požadavku, může zahrnovat hlavičky Accept.</span><span class="sxs-lookup"><span data-stu-id="75d96-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="75d96-117">Hlavička Accept informuje, že server, které média typu (typů) klienta chce ze serveru.</span><span class="sxs-lookup"><span data-stu-id="75d96-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="75d96-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="75d96-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="75d96-119">Tuto hlavičku informuje server, že klienta chce HTML, XHTML nebo XML.</span><span class="sxs-lookup"><span data-stu-id="75d96-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="75d96-120">Typ média určuje, jak webové rozhraní API serializuje a deserializuje obsah zprávy HTTP.</span><span class="sxs-lookup"><span data-stu-id="75d96-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="75d96-121">Webové rozhraní API má integrovanou podporu pro XML, JSON, BSON a data formuláře urlencoded a může podporovat dalšího média typy zápis *formátovací modul média*.</span><span class="sxs-lookup"><span data-stu-id="75d96-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="75d96-122">Pokud chcete vytvořit formátovací modul médií, odvozena od jedné z těchto tříd:</span><span class="sxs-lookup"><span data-stu-id="75d96-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="75d96-123">[Objekt MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="75d96-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="75d96-124">Tato třída používá asynchronního čtení a zápisu metody.</span><span class="sxs-lookup"><span data-stu-id="75d96-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="75d96-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="75d96-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="75d96-126">Tato třída odvozená z **objekt MediaTypeFormatter** ale používá sychronous metody pro čtení a zápis.</span><span class="sxs-lookup"><span data-stu-id="75d96-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="75d96-127">Odvozování z **BufferedMediaTypeFormatter** je jednodušší, protože neexistuje žádný asynchronní kód, ale také znamená to během vstupně-výstupních operací můžete blokovat volající vlákno.</span><span class="sxs-lookup"><span data-stu-id="75d96-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="75d96-128">Příklad: Vytvoření média formátování sdíleného svazku clusteru</span><span class="sxs-lookup"><span data-stu-id="75d96-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="75d96-129">Následující příklad ukazuje formátovací modul typu média, která může serializovat objekt produktu do formátu hodnot oddělených čárkami (CSV).</span><span class="sxs-lookup"><span data-stu-id="75d96-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="75d96-130">Tento příklad používá typ produktu definované v tomto kurzu [vytváření webového rozhraní API této operace CRUD podporuje](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="75d96-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="75d96-131">Zde je definice objektu produktu:</span><span class="sxs-lookup"><span data-stu-id="75d96-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="75d96-132">K implementaci formátování CSV, definice třídy, která je odvozena z **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="75d96-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="75d96-133">V konstruktoru přidejte typy médií podporovanými formátování podporuje.</span><span class="sxs-lookup"><span data-stu-id="75d96-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="75d96-134">V tomto příkladu formátování podporuje typu jedno médium, &quot;text/csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="75d96-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="75d96-135">Přepsání **CanWriteType** může serializovat metoda k označení, které typy formátování:</span><span class="sxs-lookup"><span data-stu-id="75d96-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="75d96-136">V tomto příkladu může serializovat jedné formátovací modul `Product` objekty a také kolekce `Product` objekty.</span><span class="sxs-lookup"><span data-stu-id="75d96-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="75d96-137">Podobně přepsat **CanReadType** metoda k označení, které typy formátovací modul může deserializovat.</span><span class="sxs-lookup"><span data-stu-id="75d96-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="75d96-138">V tomto příkladu formátování nepodporuje deserializace, takže jednoduše vrátí metoda **false**.</span><span class="sxs-lookup"><span data-stu-id="75d96-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="75d96-139">Nakonec přepsání **WriteToStream** metoda.</span><span class="sxs-lookup"><span data-stu-id="75d96-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="75d96-140">Tato metoda serializuje typu pomocí zápisu do datového proudu.</span><span class="sxs-lookup"><span data-stu-id="75d96-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="75d96-141">Pokud vaše formátovací modul podporuje deserializace, také přepsat **ReadFromStream** metoda.</span><span class="sxs-lookup"><span data-stu-id="75d96-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="75d96-142">Přidání média formátování do kanálu webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="75d96-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="75d96-143">Přidat typy médií formátovacího modulu do kanálu webové rozhraní API, použijte **formátování** vlastnost **HttpConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="75d96-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="75d96-144">Kódování znaků</span><span class="sxs-lookup"><span data-stu-id="75d96-144">Character Encodings</span></span>

<span data-ttu-id="75d96-145">Formátovací modul média může volitelně podporovat více kódování znaků, jako je například UTF-8 nebo ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="75d96-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="75d96-146">V konstruktoru, přidejte jeden nebo více [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) typů, který **SupportedEncodings** kolekce.</span><span class="sxs-lookup"><span data-stu-id="75d96-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="75d96-147">Uveďte výchozí kódování první.</span><span class="sxs-lookup"><span data-stu-id="75d96-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="75d96-148">V **WriteToStream** a **ReadFromStream** volání metody, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) vyberte kódování upřednostňované znaků.</span><span class="sxs-lookup"><span data-stu-id="75d96-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="75d96-149">Tato metoda odpovídá hlavičky žádosti podle seznamu podporovaných kódování.</span><span class="sxs-lookup"><span data-stu-id="75d96-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="75d96-150">Pomocí vráceného **kódování** při čtení nebo zápisu do datového proudu:</span><span class="sxs-lookup"><span data-stu-id="75d96-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
