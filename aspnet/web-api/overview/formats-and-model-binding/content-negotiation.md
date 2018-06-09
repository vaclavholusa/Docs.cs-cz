---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: Obsahu vyjednávání v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Popisuje, jak rozhraní ASP.NET Web API implementuje vyjednávání obsahu HTTP.
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
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566410"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="549e8-103">Vyjednávání obsahu v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="549e8-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="549e8-104">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="549e8-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="549e8-105">Tento článek popisuje, jak rozhraní ASP.NET Web API implementuje vyjednávání obsahu.</span><span class="sxs-lookup"><span data-stu-id="549e8-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="549e8-106">Specifikace protokolu HTTP (RFC 2616) definuje vyjednávání obsahu jako "proces výběru nejlepší reprezentace pro dané odpovědi, když jsou k dispozici více reprezentace."</span><span class="sxs-lookup"><span data-stu-id="549e8-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="549e8-107">Primární mechanismus pro vyjednávání obsahu protokolu HTTP jsou tyto hlavičky požadavku:</span><span class="sxs-lookup"><span data-stu-id="549e8-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="549e8-108">**Přijměte:** typů médií, které jsou přijatelné pro odpovědi, například "application/json", "application/xml" nebo typ vlastní média, jako &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="549e8-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="549e8-109">**Accept-Charset:** které znakové sady jsou přijatelné, jako je například UTF-8 nebo ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="549e8-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="549e8-110">**Přijměte-Encoding:** které kódování obsahu jsou přijatelné, například gzip.</span><span class="sxs-lookup"><span data-stu-id="549e8-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="549e8-111">**Přijmout jazyk:** upřednostňované přirozeného jazyka, jako "en-us".</span><span class="sxs-lookup"><span data-stu-id="549e8-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="549e8-112">Server můžete také zobrazit další části požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="549e8-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="549e8-113">Například pokud požadavek obsahuje hlavičku X-Requested-With, označující požadavek AJAX serveru může výchozí JSON Pokud neexistuje žádné hlavičky Accept.</span><span class="sxs-lookup"><span data-stu-id="549e8-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="549e8-114">V tomto článku se podíváme používání hlavičky Accept a Accept-Charset webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="549e8-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="549e8-115">(V tomto okamžiku se žádné integrovanou podporu pro Accept-Encoding nebo Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="549e8-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="549e8-116">Serializace</span><span class="sxs-lookup"><span data-stu-id="549e8-116">Serialization</span></span>

<span data-ttu-id="549e8-117">Pokud kontroler Web API vrátí prostředků jako typ CLR, kanál návratovou hodnotu serializuje a zapíše do odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="549e8-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="549e8-118">Zvažte například následující akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="549e8-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="549e8-119">Klient může odesílat tento požadavek HTTP:</span><span class="sxs-lookup"><span data-stu-id="549e8-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="549e8-120">V odpovědi může odeslat na server:</span><span class="sxs-lookup"><span data-stu-id="549e8-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="549e8-121">V tomto příkladu si klient vyžádal JSON, Javascript nebo "nic" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="549e8-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="549e8-122">Server odpověď má reprezentaci JSON `Product` objektu.</span><span class="sxs-lookup"><span data-stu-id="549e8-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="549e8-123">Všimněte si, že v odpovědi hlavičky Content-Type je nastaven na &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="549e8-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="549e8-124">Můžete se taky vrátit řadič **objekt HttpResponseMessage** objektu.</span><span class="sxs-lookup"><span data-stu-id="549e8-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="549e8-125">Chcete-li zadat objekt CLR pro text odpovědi, zavolejte **CreateResponse** metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="549e8-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="549e8-126">Tato možnost vám dává větší kontrolu nad podrobnosti o odpovědi.</span><span class="sxs-lookup"><span data-stu-id="549e8-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="549e8-127">Můžete nastavit kód stavu, přidejte hlavičky protokolu HTTP a tak dále.</span><span class="sxs-lookup"><span data-stu-id="549e8-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="549e8-128">Objekt, který serializuje prostředku se nazývá *formátovací modul média*.</span><span class="sxs-lookup"><span data-stu-id="549e8-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="549e8-129">Formátovací moduly pro média odvozena od **objekt MediaTypeFormatter** třídy.</span><span class="sxs-lookup"><span data-stu-id="549e8-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="549e8-130">Webové rozhraní API poskytuje formátovací moduly pro média pro soubor XML a JSON a můžete vytvořit vlastní formátovací moduly, které podporují jiné typy médií.</span><span class="sxs-lookup"><span data-stu-id="549e8-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="549e8-131">Informace o vytváření vlastních formátování najdete v tématu [formátovací moduly pro média](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="549e8-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="549e8-132">Jak obsahu vyjednávání funguje</span><span class="sxs-lookup"><span data-stu-id="549e8-132">How Content Negotiation Works</span></span>

<span data-ttu-id="549e8-133">Nejdřív získá kanálu **IContentNegotiator** služby z **HttpConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="549e8-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="549e8-134">Také získá formátovací moduly pro média ze seznamu **HttpConfiguration.Formatters** kolekce.</span><span class="sxs-lookup"><span data-stu-id="549e8-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="549e8-135">V dalším kroku kanálu volání **IContentNegotiatior.Negotiate**a předejte:</span><span class="sxs-lookup"><span data-stu-id="549e8-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="549e8-136">Typ k serializaci objektu</span><span class="sxs-lookup"><span data-stu-id="549e8-136">The type of object to serialize</span></span>
- <span data-ttu-id="549e8-137">Kolekce formátovací moduly pro média</span><span class="sxs-lookup"><span data-stu-id="549e8-137">The collection of media formatters</span></span>
- <span data-ttu-id="549e8-138">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="549e8-138">The HTTP request</span></span>

<span data-ttu-id="549e8-139">**Negotiate** metoda vrátí dva údaje:</span><span class="sxs-lookup"><span data-stu-id="549e8-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="549e8-140">Které formátovací modul použít</span><span class="sxs-lookup"><span data-stu-id="549e8-140">Which formatter to use</span></span>
- <span data-ttu-id="549e8-141">Typ média pro odpověď</span><span class="sxs-lookup"><span data-stu-id="549e8-141">The media type for the response</span></span>

<span data-ttu-id="549e8-142">Pokud není formátování nalezeno, **Negotiate** metoda vrátí **null**a chyba recevies HTTP klienta 406 (nepřijatelný).</span><span class="sxs-lookup"><span data-stu-id="549e8-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="549e8-143">Následující kód ukazuje, jak řadič přímo vyvolat vyjednávání obsahu:</span><span class="sxs-lookup"><span data-stu-id="549e8-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="549e8-144">Tento kód je ekvivalentní co kanálu provede automaticky.</span><span class="sxs-lookup"><span data-stu-id="549e8-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="549e8-145">Vyjednavač obsahu výchozí</span><span class="sxs-lookup"><span data-stu-id="549e8-145">Default Content Negotiator</span></span>

<span data-ttu-id="549e8-146">**DefaultContentNegotiator** třída poskytuje výchozí implementaci **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="549e8-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="549e8-147">Chcete-li vybrat formátovací modul používá několik kritéria.</span><span class="sxs-lookup"><span data-stu-id="549e8-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="549e8-148">Formátovací modul nejprve musí být schopen daný typ serializovat.</span><span class="sxs-lookup"><span data-stu-id="549e8-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="549e8-149">To je ověřit voláním **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="549e8-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="549e8-150">Vyjednavač obsahu v dalším kroku vypadá na jednotlivé formátovací moduly a vyhodnotí, jak dobře odpovídá požadavek HTTP.</span><span class="sxs-lookup"><span data-stu-id="549e8-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="549e8-151">K vyhodnocení shody, vypadá vyjednavač obsahu na dvě věci na formátovací modul:</span><span class="sxs-lookup"><span data-stu-id="549e8-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="549e8-152">**SupportedMediaTypes** kolekce, který obsahuje seznam podporovanými typy médií.</span><span class="sxs-lookup"><span data-stu-id="549e8-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="549e8-153">Vyjednavač obsahu se pokusí vyhledat tento seznam u této žádosti hlavička Accept.</span><span class="sxs-lookup"><span data-stu-id="549e8-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="549e8-154">Všimněte si, že hlavičku Accept může obsahovat rozsahy.</span><span class="sxs-lookup"><span data-stu-id="549e8-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="549e8-155">Například "text/plain" je nalezena shoda pro text /\* nebo \* / \*.</span><span class="sxs-lookup"><span data-stu-id="549e8-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="549e8-156">**MediaTypeMappings** kolekce, který obsahuje seznam **MediaTypeMapping** objekty.</span><span class="sxs-lookup"><span data-stu-id="549e8-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="549e8-157">**MediaTypeMapping** třída poskytuje obecné způsob, jak porovnávají požadavky HTTP s typy médií.</span><span class="sxs-lookup"><span data-stu-id="549e8-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="549e8-158">Například je může namapovat vlastní hlavičku HTTP pro určitý typ média.</span><span class="sxs-lookup"><span data-stu-id="549e8-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="549e8-159">Pokud máte více odpovídá, shoda s nejvyšší wins koeficientu kvality.</span><span class="sxs-lookup"><span data-stu-id="549e8-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="549e8-160">Příklad:</span><span class="sxs-lookup"><span data-stu-id="549e8-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="549e8-161">V tomto příkladu application/json má faktor předpokládané kvality 1.0, takže je upřednostňovaný nad application/xml.</span><span class="sxs-lookup"><span data-stu-id="549e8-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="549e8-162">Pokud nejsou nalezeny žádné shody, vyjednavač obsahu pokusí porovnávaný typ média obsahu žádosti, pokud existuje.</span><span class="sxs-lookup"><span data-stu-id="549e8-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="549e8-163">Například pokud požadavek obsahuje JSON data, vyjednavač obsahu vypadá formátování JSON.</span><span class="sxs-lookup"><span data-stu-id="549e8-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="549e8-164">Pokud ještě nejsou žádné shody, vyjednavač obsahu jednoduše vybere první formátovací modul, který dokáže daný typ serializovat.</span><span class="sxs-lookup"><span data-stu-id="549e8-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="549e8-165">Výběr kódování znaků</span><span class="sxs-lookup"><span data-stu-id="549e8-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="549e8-166">Po výběru formátování se vyjednavač obsahu vybere nejvhodnější kódování znaků prohlížením **SupportedEncodings** vlastnost formátování a odpovídající proti hlavičku Accept-Charset v požadavku (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="549e8-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
