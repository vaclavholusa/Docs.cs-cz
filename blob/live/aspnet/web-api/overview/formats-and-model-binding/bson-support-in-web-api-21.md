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
ms.openlocfilehash: 08ef1564b2f8f11294c3bb1ec0ff9a3d063895b6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="a4608-102">BSON podpora v rozhraní ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="a4608-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="a4608-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a4608-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a4608-104">Webové rozhraní API 2.1 zavádí podporu pro BSON.</span><span class="sxs-lookup"><span data-stu-id="a4608-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="a4608-105">Toto téma ukazuje, jak použít BSON do kontroleru webového rozhraní API (na straně serveru) a v klientské aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="a4608-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="a4608-106">Co je BSON?</span><span class="sxs-lookup"><span data-stu-id="a4608-106">What is BSON?</span></span>

<span data-ttu-id="a4608-107">[BSON](http://bsonspec.org/) je formát binární serializace.</span><span class="sxs-lookup"><span data-stu-id="a4608-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="a4608-108">"BSON" znamená "Binární JSON", ale jsou velmi jinak serializovat formátů BSON a JSON.</span><span class="sxs-lookup"><span data-stu-id="a4608-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="a4608-109">Je BSON "JSON jako", protože objekty jsou reprezentovány jako dvojice název hodnota, podobně jako JSON.</span><span class="sxs-lookup"><span data-stu-id="a4608-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="a4608-110">Na rozdíl od formát JSON číselné datové typy jsou uložena bajtů, není řetězce</span><span class="sxs-lookup"><span data-stu-id="a4608-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="a4608-111">BSON byla navržená tak, aby byly lightweight, proletět a rychlé kódování nebo dekódování.</span><span class="sxs-lookup"><span data-stu-id="a4608-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="a4608-112">BSON je srovnatelná velikost do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="a4608-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="a4608-113">V závislosti na data může být datovou část BSON menší nebo větší než datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="a4608-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="a4608-114">Pro serializaci binárních dat, jako soubor obrázku BSON je menší než JSON, protože neobsahuje binární data není kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="a4608-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data does is not base64-encoded.</span></span>
- <span data-ttu-id="a4608-115">BSON dokumenty lze snadno kontrolu, protože elementy mají předponu pole Délka, takže analyzátor, můžete přeskočit elementy bez dekódování je.</span><span class="sxs-lookup"><span data-stu-id="a4608-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="a4608-116">Kódování a dekódování jsou efektivní, protože číselné datové typy jsou uloženy jako čísla, řetězce není.</span><span class="sxs-lookup"><span data-stu-id="a4608-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="a4608-117">Nativní klienty, například klientské aplikace .NET, můžete využívat BSON místo založený na textu formátů například JSON a XML.</span><span class="sxs-lookup"><span data-stu-id="a4608-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="a4608-118">Pro klienty prohlížeče budete pravděpodobně chtít přilepit s formát JSON, protože JavaScript můžete přímo převést datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="a4608-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="a4608-119">Naštěstí webového rozhraní API používá [vyjednávání obsahu](content-negotiation.md), takže vaše rozhraní API podporuje oba formáty a zvolte klienta.</span><span class="sxs-lookup"><span data-stu-id="a4608-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="a4608-120">Povolení BSON na serveru</span><span class="sxs-lookup"><span data-stu-id="a4608-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="a4608-121">V konfiguraci vašeho webového rozhraní API, přidejte **BsonMediaTypeFormatter** do kolekce formátování.</span><span class="sxs-lookup"><span data-stu-id="a4608-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="a4608-122">Pokud klient požaduje "application/bson", teď webového rozhraní API použije formátování BSON.</span><span class="sxs-lookup"><span data-stu-id="a4608-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="a4608-123">Chcete-li BSON přidružit jiné typy médií, přidejte ho do kolekci SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="a4608-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="a4608-124">Následující kód přidá "application/vnd.contoso" typech podporovaných médií:</span><span class="sxs-lookup"><span data-stu-id="a4608-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="a4608-125">Příklad relaci HTTP</span><span class="sxs-lookup"><span data-stu-id="a4608-125">Example HTTP Session</span></span>

<span data-ttu-id="a4608-126">V tomto příkladu použijeme následující třídy modelu plus jednoduché kontroleru webového rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="a4608-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="a4608-127">Klient může odesílat následující požadavek HTTP:</span><span class="sxs-lookup"><span data-stu-id="a4608-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="a4608-128">Tady je odpovědi:</span><span class="sxs-lookup"><span data-stu-id="a4608-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="a4608-129">Zde I jste nahradit binární data s &quot;.&quot; znaků.</span><span class="sxs-lookup"><span data-stu-id="a4608-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="a4608-130">Následující snímek obrazovky z Fiddler ukazuje nezpracované šestnáctkové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a4608-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="a4608-131">Pomocí BSON HttpClient</span><span class="sxs-lookup"><span data-stu-id="a4608-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="a4608-132">Klienti aplikace .NET můžete použít formátovací modul BSON s **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="a4608-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="a4608-133">Další informace o **HttpClient**, najdete v části [volání webového rozhraní API z klienta .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="a4608-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="a4608-134">Následující kód odešle požadavek GET, který přijímá BSON a pak deserializuje datovou část BSON v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="a4608-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="a4608-135">Chcete-li požadavek BSON ze serveru, nastavte hlavičku Accept pro "application/bson":</span><span class="sxs-lookup"><span data-stu-id="a4608-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="a4608-136">K deserializaci text odpovědi, použijte **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="a4608-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="a4608-137">Tento formátovací modul není v kolekci výchozí formátování, takže budete muset zadat při čtení textu odpovědi na:</span><span class="sxs-lookup"><span data-stu-id="a4608-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="a4608-138">Další příklad ukazuje, jak odeslat požadavek POST, který obsahuje BSON.</span><span class="sxs-lookup"><span data-stu-id="a4608-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="a4608-139">Velká část tento kód je stejný jako předchozí příklad.</span><span class="sxs-lookup"><span data-stu-id="a4608-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="a4608-140">Ale v **PostAsync** metoda, zadejte **BsonMediaTypeFormatter** jako formátovací modul:</span><span class="sxs-lookup"><span data-stu-id="a4608-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="a4608-141">Serializace nejvyšší úrovně primitivní typy</span><span class="sxs-lookup"><span data-stu-id="a4608-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="a4608-142">Každý dokument BSON je seznam párů klíč/hodnota. Specifikace BSON nedefinuje syntaxe pro serializaci jednu nezpracovanou hodnotu, jako je například celé číslo nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="a4608-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="a4608-143">Chcete-li toto omezení obejít **BsonMediaTypeFormatter** považuje za zvláštní případ primitivní typy.</span><span class="sxs-lookup"><span data-stu-id="a4608-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="a4608-144">Před serializaci, se převede hodnotu na dvojici klíč/hodnota s klíčem "Value".</span><span class="sxs-lookup"><span data-stu-id="a4608-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="a4608-145">Předpokládejme například, že řadiči rozhraní API vrátí celé číslo:</span><span class="sxs-lookup"><span data-stu-id="a4608-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="a4608-146">Před serializaci, převede formátování BSON to na následující dvojice klíč/hodnota:</span><span class="sxs-lookup"><span data-stu-id="a4608-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="a4608-147">Při deserializaci, převede formátování data zpět na původní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a4608-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="a4608-148">Však klienti, kteří používají jiný analyzátor BSON muset zpracovávat tento případ, pokud webového rozhraní API vrátí nezpracované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a4608-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="a4608-149">Obecně byste měli zvážit, vrácení strukturovaných dat, nikoli nezpracované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a4608-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a4608-150">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="a4608-150">Additional Resources</span></span>

[<span data-ttu-id="a4608-151">Ukázkové webové rozhraní API BSON</span><span class="sxs-lookup"><span data-stu-id="a4608-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="a4608-152">Formátovací moduly pro média</span><span class="sxs-lookup"><span data-stu-id="a4608-152">Media Formatters</span></span>](media-formatters.md)
