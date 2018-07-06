---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: V rozhraní ASP.NET Web API vyjednávání obsahu | Dokumentace Microsoftu
author: MikeWasson
description: Popisuje, jak rozhraní ASP.NET Web API implementuje vyjednávání obsahu HTTP.
ms.author: aspnetcontent
ms.date: 05/20/2012
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: 2314a263a12c74e80c08391ae03425955a82458a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810314"
---
<a name="content-negotiation-in-aspnet-web-api"></a><span data-ttu-id="21fa5-103">Vyjednávání obsahu v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="21fa5-103">Content Negotiation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="21fa5-104">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="21fa5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="21fa5-105">Tento článek popisuje, jak rozhraní ASP.NET Web API implementuje vyjednávání obsahu.</span><span class="sxs-lookup"><span data-stu-id="21fa5-105">This article describes how ASP.NET Web API implements content negotiation.</span></span>

<span data-ttu-id="21fa5-106">Specifikace protokolu HTTP (RFC 2616) definuje vyjednávání obsahu jako "proces výběru nejlepší reprezentaci pro danou odpověď, když jsou k dispozici více reprezentací."</span><span class="sxs-lookup"><span data-stu-id="21fa5-106">The HTTP specification (RFC 2616) defines content negotiation as "the process of selecting the best representation for a given response when there are multiple representations available."</span></span> <span data-ttu-id="21fa5-107">Hlavní mechanismus pro vyjednávání obsahu protokolu HTTP jsou tyto hlavičky požadavku:</span><span class="sxs-lookup"><span data-stu-id="21fa5-107">The primary mechanism for content negotiation in HTTP are these request headers:</span></span>

- <span data-ttu-id="21fa5-108">**Přijměte:** typy médií, které jsou přijatelné pro odpovědi, například "application/json", "application/xml" nebo vlastního typu, jako &quot;application/vnd.example+xml&quot;</span><span class="sxs-lookup"><span data-stu-id="21fa5-108">**Accept:** Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as &quot;application/vnd.example+xml&quot;</span></span>
- <span data-ttu-id="21fa5-109">**Přijměte-Charset:** které znakové sady jsou přijatelné, například UTF-8 nebo ISO 8859-1.</span><span class="sxs-lookup"><span data-stu-id="21fa5-109">**Accept-Charset:** Which character sets are acceptable, such as UTF-8 or ISO 8859-1.</span></span>
- <span data-ttu-id="21fa5-110">**Přijmout kódování:** které kódování obsahu jsou přijatelné, například gzip.</span><span class="sxs-lookup"><span data-stu-id="21fa5-110">**Accept-Encoding:** Which content encodings are acceptable, such as gzip.</span></span>
- <span data-ttu-id="21fa5-111">**Přijmout jazyka:** upřednostňované přirozeného jazyka, jako například "en-nám".</span><span class="sxs-lookup"><span data-stu-id="21fa5-111">**Accept-Language:** The preferred natural language, such as "en-us".</span></span>

<span data-ttu-id="21fa5-112">Server můžete se také podívat na další části požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="21fa5-112">The server can also look at other portions of the HTTP request.</span></span> <span data-ttu-id="21fa5-113">Například pokud požadavek obsahuje hlavičku X-Requested-With, označující požadavek AJAX na serveru může být jako výchozí JSON Pokud neexistuje žádné hlavičky Accept.</span><span class="sxs-lookup"><span data-stu-id="21fa5-113">For example, if the request contains an X-Requested-With header, indicating an AJAX request, the server might default to JSON if there is no Accept header.</span></span>

<span data-ttu-id="21fa5-114">V tomto článku se podíváme na použití hlavičky Accept a Accept-Charset webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="21fa5-114">In this article, we'll look at how Web API uses the Accept and Accept-Charset headers.</span></span> <span data-ttu-id="21fa5-115">(V tuto chvíli není předdefinovanou podporu Accept-Encoding nebo Accept-Language.)</span><span class="sxs-lookup"><span data-stu-id="21fa5-115">(At this time, there is no built-in support for Accept-Encoding or Accept-Language.)</span></span>

## <a name="serialization"></a><span data-ttu-id="21fa5-116">Serializace</span><span class="sxs-lookup"><span data-stu-id="21fa5-116">Serialization</span></span>

<span data-ttu-id="21fa5-117">Pokud kontroler webového rozhraní API vrátí prostředků jako typ CLR, kanál serializuje návratovou hodnotu a zapisuje je do datové části odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="21fa5-117">If a Web API controller returns a resource as CLR type, the pipeline serializes the return value and writes it into the HTTP response body.</span></span>

<span data-ttu-id="21fa5-118">Představte si třeba následující akce kontroleru:</span><span class="sxs-lookup"><span data-stu-id="21fa5-118">For example, consider the following controller action:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

<span data-ttu-id="21fa5-119">Klient může odeslat požadavek protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="21fa5-119">A client might send this HTTP request:</span></span>

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

<span data-ttu-id="21fa5-120">V odpovědi může server odeslat:</span><span class="sxs-lookup"><span data-stu-id="21fa5-120">In response, the server might send:</span></span>

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

<span data-ttu-id="21fa5-121">V tomto příkladu klient vyžádal JSON, Javascript nebo "nic" (\*/\*).</span><span class="sxs-lookup"><span data-stu-id="21fa5-121">In this example, the client requested either JSON, Javascript, or "anything" (\*/\*).</span></span> <span data-ttu-id="21fa5-122">Server odpověď s JSON s reprezentací provedených `Product` objektu.</span><span class="sxs-lookup"><span data-stu-id="21fa5-122">The server responsed with a JSON representation of the `Product` object.</span></span> <span data-ttu-id="21fa5-123">Všimněte si, že hlavička Content-Type v odpovědi nastavena na &quot;application/json&quot;.</span><span class="sxs-lookup"><span data-stu-id="21fa5-123">Notice that the Content-Type header in the response is set to &quot;application/json&quot;.</span></span>

<span data-ttu-id="21fa5-124">Kontroler může také vrátit **objekt HttpResponseMessage** objektu.</span><span class="sxs-lookup"><span data-stu-id="21fa5-124">A controller can also return an **HttpResponseMessage** object.</span></span> <span data-ttu-id="21fa5-125">Pokud chcete nastavit objektu CLR pro tělo odpovědi, zavolejte **CreateResponse** – metoda rozšíření:</span><span class="sxs-lookup"><span data-stu-id="21fa5-125">To specify a CLR object for the response body, call the **CreateResponse** extension method:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

<span data-ttu-id="21fa5-126">Tato možnost nabízí větší kontrolu nad podrobnosti odpovědi.</span><span class="sxs-lookup"><span data-stu-id="21fa5-126">This option gives you more control over the details of the response.</span></span> <span data-ttu-id="21fa5-127">Stavový kód, můžete nastavit přidání hlavičky protokolu HTTP a tak dále.</span><span class="sxs-lookup"><span data-stu-id="21fa5-127">You can set the status code, add HTTP headers, and so forth.</span></span>

<span data-ttu-id="21fa5-128">Objekt, který serializuje prostředek, se nazývá *formátovací modul médií*.</span><span class="sxs-lookup"><span data-stu-id="21fa5-128">The object that serializes the resource is called a *media formatter*.</span></span> <span data-ttu-id="21fa5-129">Formátovací moduly médií odvozovat **objekt MediaTypeFormatter** třídy.</span><span class="sxs-lookup"><span data-stu-id="21fa5-129">Media formatters derive from the **MediaTypeFormatter** class.</span></span> <span data-ttu-id="21fa5-130">Webové rozhraní API poskytuje formátovací moduly médií pro XML a JSON, a můžete vytvořit vlastní formátovací moduly pro podporu jiných typů médií.</span><span class="sxs-lookup"><span data-stu-id="21fa5-130">Web API provides media formatters for XML and JSON, and you can create custom formatters to support other media types.</span></span> <span data-ttu-id="21fa5-131">Informace o tvorbě vlastní formátování najdete v tématu [formátovací moduly médií](media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="21fa5-131">For information about writing a custom formatter, see [Media Formatters](media-formatters.md).</span></span>

## <a name="how-content-negotiation-works"></a><span data-ttu-id="21fa5-132">Jak obsahu vyjednávání funguje</span><span class="sxs-lookup"><span data-stu-id="21fa5-132">How Content Negotiation Works</span></span>

<span data-ttu-id="21fa5-133">Nejprve kanál získá **IContentNegotiator** služba **HttpConfiguration** objektu.</span><span class="sxs-lookup"><span data-stu-id="21fa5-133">First, the pipeline gets the **IContentNegotiator** service from the **HttpConfiguration** object.</span></span> <span data-ttu-id="21fa5-134">Také získá formátovací moduly médií ze seznamu **HttpConfiguration.Formatters** kolekce.</span><span class="sxs-lookup"><span data-stu-id="21fa5-134">It also gets the list of media formatters from the **HttpConfiguration.Formatters** collection.</span></span>

<span data-ttu-id="21fa5-135">Dále volá kanál **IContentNegotiatior.Negotiate**a předejte:</span><span class="sxs-lookup"><span data-stu-id="21fa5-135">Next, the pipeline calls **IContentNegotiatior.Negotiate**, passing in:</span></span>

- <span data-ttu-id="21fa5-136">Typ objektu určeného k serializaci</span><span class="sxs-lookup"><span data-stu-id="21fa5-136">The type of object to serialize</span></span>
- <span data-ttu-id="21fa5-137">Kolekce formátovací moduly médií</span><span class="sxs-lookup"><span data-stu-id="21fa5-137">The collection of media formatters</span></span>
- <span data-ttu-id="21fa5-138">Požadavek HTTP</span><span class="sxs-lookup"><span data-stu-id="21fa5-138">The HTTP request</span></span>

<span data-ttu-id="21fa5-139">**Negotiate** metoda vrací dva druhy údajů:</span><span class="sxs-lookup"><span data-stu-id="21fa5-139">The **Negotiate** method returns two pieces of information:</span></span>

- <span data-ttu-id="21fa5-140">Formátování, které použito</span><span class="sxs-lookup"><span data-stu-id="21fa5-140">Which formatter to use</span></span>
- <span data-ttu-id="21fa5-141">Typ média pro odpověď</span><span class="sxs-lookup"><span data-stu-id="21fa5-141">The media type for the response</span></span>

<span data-ttu-id="21fa5-142">Pokud není formátování nalezeno, **Negotiate** vrátí metoda **null**a chyba klienta recevies HTTP 406 (nepřijatelné).</span><span class="sxs-lookup"><span data-stu-id="21fa5-142">If no formatter is found, the **Negotiate** method returns **null**, and the client recevies HTTP error 406 (Not Acceptable).</span></span>

<span data-ttu-id="21fa5-143">Následující kód ukazuje, jak můžete kontroler přímo vyvolat vyjednávání obsahu:</span><span class="sxs-lookup"><span data-stu-id="21fa5-143">The following code shows how a controller can directly invoke content negotiation:</span></span>

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

<span data-ttu-id="21fa5-144">Tento kód je ekvivalentní k co kanál provádí automaticky.</span><span class="sxs-lookup"><span data-stu-id="21fa5-144">This code is equivalent to the what the pipeline does automatically.</span></span>

## <a name="default-content-negotiator"></a><span data-ttu-id="21fa5-145">Výchozí Negotiator obsahu</span><span class="sxs-lookup"><span data-stu-id="21fa5-145">Default Content Negotiator</span></span>

<span data-ttu-id="21fa5-146">**DefaultContentNegotiator** třída poskytuje výchozí implementaci třídy **IContentNegotiator**.</span><span class="sxs-lookup"><span data-stu-id="21fa5-146">The **DefaultContentNegotiator** class provides the default implementation of **IContentNegotiator**.</span></span> <span data-ttu-id="21fa5-147">Vyberte formátovací modul používá několik kritérií.</span><span class="sxs-lookup"><span data-stu-id="21fa5-147">It uses several criteria to select a formatter.</span></span>

<span data-ttu-id="21fa5-148">Formátovací modul, nejprve musí být schopen daný typ serializovat.</span><span class="sxs-lookup"><span data-stu-id="21fa5-148">First, the formatter must be able to serialize the type.</span></span> <span data-ttu-id="21fa5-149">To je ověřený pomocí volání **MediaTypeFormatter.CanWriteType**.</span><span class="sxs-lookup"><span data-stu-id="21fa5-149">This is verified by calling **MediaTypeFormatter.CanWriteType**.</span></span>

<span data-ttu-id="21fa5-150">Vyjednavač obsahu dále zjistí jednotlivé formátovací moduly a vyhodnotí, jak dobře odpovídá požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="21fa5-150">Next, the content negotiator looks at each formatter and evaluates how well it matches the HTTP request.</span></span> <span data-ttu-id="21fa5-151">K vyhodnocení shody, vyjednavače obsahu prohledá dvě věci ve formátování:</span><span class="sxs-lookup"><span data-stu-id="21fa5-151">To evaluate the match, the content negotiator looks at two things on the formatter:</span></span>

- <span data-ttu-id="21fa5-152">**SupportedMediaTypes** kolekce, která obsahuje seznam podporovanými typy médií.</span><span class="sxs-lookup"><span data-stu-id="21fa5-152">The **SupportedMediaTypes** collection, which contains a list of supported media types.</span></span> <span data-ttu-id="21fa5-153">Vyjednavač obsahu se pokusí shodovat se tento seznam u této žádosti hlavičky Accept.</span><span class="sxs-lookup"><span data-stu-id="21fa5-153">The content negotiator tries to match this list against the request Accept header.</span></span> <span data-ttu-id="21fa5-154">Všimněte si, že může obsahovat hlavičku Accept rozsahy.</span><span class="sxs-lookup"><span data-stu-id="21fa5-154">Note that the Accept header can include ranges.</span></span> <span data-ttu-id="21fa5-155">Například "text/plain" je nalezena shoda s text /\* nebo \* / \*.</span><span class="sxs-lookup"><span data-stu-id="21fa5-155">For example, "text/plain" is a match for text/\* or \*/\*.</span></span>
- <span data-ttu-id="21fa5-156">**MediaTypeMappings** kolekce, která obsahuje seznam **MediaTypeMapping** objekty.</span><span class="sxs-lookup"><span data-stu-id="21fa5-156">The **MediaTypeMappings** collection, which contains a list of **MediaTypeMapping** objects.</span></span> <span data-ttu-id="21fa5-157">**MediaTypeMapping** třída poskytuje obecný způsob, jak porovnávají požadavky HTTP s typy médií.</span><span class="sxs-lookup"><span data-stu-id="21fa5-157">The **MediaTypeMapping** class provides a generic way to match HTTP requests with media types.</span></span> <span data-ttu-id="21fa5-158">Je třeba namapovat vlastní hlavičku HTTP pro určitý typ média.</span><span class="sxs-lookup"><span data-stu-id="21fa5-158">For example, it could map a custom HTTP header to a particular media type.</span></span>

<span data-ttu-id="21fa5-159">Pokud existuje více odpovídá, shoda s nejvyšší kvality faktor wins.</span><span class="sxs-lookup"><span data-stu-id="21fa5-159">If there are multiple matches, the match with the highest quality factor wins.</span></span> <span data-ttu-id="21fa5-160">Příklad:</span><span class="sxs-lookup"><span data-stu-id="21fa5-160">For example:</span></span>

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

<span data-ttu-id="21fa5-161">V tomto příkladu má application/json faktor implicitní kvality 1.0, takže je upřednostňované nad application/xml.</span><span class="sxs-lookup"><span data-stu-id="21fa5-161">In this example, application/json has an implied quality factor of 1.0, so it is preferred over application/xml.</span></span>

<span data-ttu-id="21fa5-162">Pokud nejsou nalezeny žádné shody, vyjednavače obsahu pokusí porovnávaný typ média obsahu žádosti případné.</span><span class="sxs-lookup"><span data-stu-id="21fa5-162">If no matches are found, the content negotiator tries to match on the media type of the request body, if any.</span></span> <span data-ttu-id="21fa5-163">Například pokud požadavek obsahuje JSON data, vyjednavače obsahu vypadá formátování JSON.</span><span class="sxs-lookup"><span data-stu-id="21fa5-163">For example, if the request contains JSON data, the content negotiator looks for a JSON formatter.</span></span>

<span data-ttu-id="21fa5-164">Pokud ještě neexistují žádné odpovídající položky, vyjednavače obsahu jednoduše vybere první formátovací modul, který dokáže daný typ serializovat.</span><span class="sxs-lookup"><span data-stu-id="21fa5-164">If there are still no matches, the content negotiator simply picks the first formatter that can serialize the type.</span></span>

## <a name="selecting-a-character-encoding"></a><span data-ttu-id="21fa5-165">Výběr kódování znaků</span><span class="sxs-lookup"><span data-stu-id="21fa5-165">Selecting a Character Encoding</span></span>

<span data-ttu-id="21fa5-166">Po výběru formátovacího modulu vyjednavače obsahu zvolí nejvhodnější kódování znaků pohledem **SupportedEncodings** vlastnosti formátování a shod pro hlavičku Accept-Charset v požadavku (pokud existuje).</span><span class="sxs-lookup"><span data-stu-id="21fa5-166">After a formatter is selected, the content negotiator chooses the best character encoding by looking at the **SupportedEncodings** property on the formatter, and matching it against the Accept-Charset header in the request (if any).</span></span>
