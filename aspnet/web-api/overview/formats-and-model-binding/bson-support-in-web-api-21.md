---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Podpora formátu BSON ve webovém rozhraní API 2.1 technologie ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7c5e763d92295a83b3431e9ec6e305b07f8a64d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370491"
---
<a name="bson-support-in-aspnet-web-api-21"></a><span data-ttu-id="9116e-102">Podpora formátu BSON ve webovém rozhraní API 2.1 technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9116e-102">BSON Support in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="9116e-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9116e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9116e-104">Webové rozhraní API 2.1 přináší podporu pro BSON.</span><span class="sxs-lookup"><span data-stu-id="9116e-104">Web API 2.1 introduces support for BSON.</span></span> <span data-ttu-id="9116e-105">Toto téma ukazuje, jak použít BSON ve vaší kontroler Web API (na straně serveru) a v klientské aplikaci .NET.</span><span class="sxs-lookup"><span data-stu-id="9116e-105">This topic shows how to use BSON in your Web API controller (server side) and in a .NET client app.</span></span>

## <a name="what-is-bson"></a><span data-ttu-id="9116e-106">Co je BSON?</span><span class="sxs-lookup"><span data-stu-id="9116e-106">What is BSON?</span></span>

<span data-ttu-id="9116e-107">[BSON](http://bsonspec.org/) je binární serializační formát.</span><span class="sxs-lookup"><span data-stu-id="9116e-107">[BSON](http://bsonspec.org/) is a binary serialization format.</span></span> <span data-ttu-id="9116e-108">"BSON" znamená "Binární JSON", ale formátů BSON a JSON serializují nějak významně odlišně.</span><span class="sxs-lookup"><span data-stu-id="9116e-108">"BSON" stands for "Binary JSON", but BSON and JSON are serialized very differently.</span></span> <span data-ttu-id="9116e-109">BSON je "Podobných formátu JSON", protože objekty jsou reprezentovány ve formě dvojice název hodnota, podobně jako JSON.</span><span class="sxs-lookup"><span data-stu-id="9116e-109">BSON is "JSON-like", because objects are represented as name-value pairs, similar to JSON.</span></span> <span data-ttu-id="9116e-110">Na rozdíl od JSON číselné datové typy jsou uloženy jako bajtů, ne řetězce</span><span class="sxs-lookup"><span data-stu-id="9116e-110">Unlike JSON, numeric data types are stored as bytes, not strings</span></span>

<span data-ttu-id="9116e-111">BSON je navržený tak, že byly zjednodušené, rychle proletět a rychlé kódování/dekódování.</span><span class="sxs-lookup"><span data-stu-id="9116e-111">BSON was designed to be lightweight, easy to scan, and fast to encode/decode.</span></span>

- <span data-ttu-id="9116e-112">BSON je srovnatelné velikosti do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="9116e-112">BSON is comparable in size to JSON.</span></span> <span data-ttu-id="9116e-113">V závislosti na data může být datová část BSON menší nebo větší než datovou část JSON.</span><span class="sxs-lookup"><span data-stu-id="9116e-113">Depending on the data, a BSON payload may be smaller or larger than a JSON payload.</span></span> <span data-ttu-id="9116e-114">Pro serializaci binárních dat, jako je například soubor obrázku je BSON menší než JSON, protože není binární data s kódováním base64.</span><span class="sxs-lookup"><span data-stu-id="9116e-114">For serializing binary data, such as an image file, BSON is smaller than JSON, because the binary data is not base64-encoded.</span></span>
- <span data-ttu-id="9116e-115">BSON dokumenty jsou usnadňuje kontrolu, protože elementy mají předponu pole length, tak analyzátor mohli přejít rovnou elementy bez dekódování je.</span><span class="sxs-lookup"><span data-stu-id="9116e-115">BSON documents are easy to scan because elements are prefixed with a length field, so a parser can skip elements without decoding them.</span></span>
- <span data-ttu-id="9116e-116">Kódování a dekódování jsou efektivní, protože číselné datové typy jsou uloženy jako čísla, řetězce není.</span><span class="sxs-lookup"><span data-stu-id="9116e-116">Encoding and decoding are efficient, because numeric data types are stored as numbers, not strings.</span></span>

<span data-ttu-id="9116e-117">Nativní klienty, jako je například klientské aplikace .NET, můžete s výhodou využít BSON místo textové formáty, jako je JSON nebo XML.</span><span class="sxs-lookup"><span data-stu-id="9116e-117">Native clients, such as .NET client apps, can benefit from using BSON in place of text-based formats such as JSON or XML.</span></span> <span data-ttu-id="9116e-118">Pro klienty prohlížeče bude pravděpodobně chcete zůstat u JSON, protože JavaScript můžete přímo převést datovou část JSON.</span><span class="sxs-lookup"><span data-stu-id="9116e-118">For browser clients, you will probably want to stick with JSON, because JavaScript can directly convert the JSON payload.</span></span>

<span data-ttu-id="9116e-119">Naštěstí webového rozhraní API používá [vyjednávání obsahu](content-negotiation.md), takže můžete podporují oba formáty a umožnit klienta, vyberte rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="9116e-119">Fortunately, Web API uses [content negotiation](content-negotiation.md), so your API can support both formats and let the client choose.</span></span>

## <a name="enabling-bson-on-the-server"></a><span data-ttu-id="9116e-120">Povolení BSON na serveru</span><span class="sxs-lookup"><span data-stu-id="9116e-120">Enabling BSON on the Server</span></span>

<span data-ttu-id="9116e-121">V konfiguraci webového rozhraní API, přidejte **BsonMediaTypeFormatter** do kolekce formátovacích modulů.</span><span class="sxs-lookup"><span data-stu-id="9116e-121">In your Web API configuration, add the **BsonMediaTypeFormatter** to the formatters collection.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

<span data-ttu-id="9116e-122">Pokud si klient vyžádá "application/bson", teď webové rozhraní API použije BSON formátovací modul.</span><span class="sxs-lookup"><span data-stu-id="9116e-122">Now if the client requests "application/bson", Web API will use the BSON formatter.</span></span>

<span data-ttu-id="9116e-123">BSON přidružit jiné typy médií, přidejte je do kolekce SupportedMediaTypes.</span><span class="sxs-lookup"><span data-stu-id="9116e-123">To associate BSON with other media types, add them to the SupportedMediaTypes collection.</span></span> <span data-ttu-id="9116e-124">Následující kód přidá do typů médií podporovaných "application/vnd.contoso":</span><span class="sxs-lookup"><span data-stu-id="9116e-124">The following code adds "application/vnd.contoso" to the supported media types:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a><span data-ttu-id="9116e-125">Příklad relace HTTP</span><span class="sxs-lookup"><span data-stu-id="9116e-125">Example HTTP Session</span></span>

<span data-ttu-id="9116e-126">V tomto příkladu použijeme následující třídy modelu a jednoduché kontroler Web API:</span><span class="sxs-lookup"><span data-stu-id="9116e-126">For this example, we'll use the following model class plus a simple Web API controller:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

<span data-ttu-id="9116e-127">Klient může odesílat následující požadavek protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="9116e-127">A client might send the following HTTP request:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

<span data-ttu-id="9116e-128">Tady je odpovědi:</span><span class="sxs-lookup"><span data-stu-id="9116e-128">Here is the response:</span></span>

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

<span data-ttu-id="9116e-129">Tady mám jsme nahradit binární data s využitím &quot;.&quot; znaků.</span><span class="sxs-lookup"><span data-stu-id="9116e-129">Here I've replaced the binary data with &quot;.&quot; characters.</span></span> <span data-ttu-id="9116e-130">Následující snímek obrazovky z Fiddleru zobrazí nezpracované šestnáctkové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9116e-130">The following screen shot from Fiddler shows the raw hex values.</span></span>

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a><span data-ttu-id="9116e-131">BSON pomocí HttpClient</span><span class="sxs-lookup"><span data-stu-id="9116e-131">Using BSON with HttpClient</span></span>

<span data-ttu-id="9116e-132">Klienti aplikací .NET můžete použít formátování BSON s **HttpClient**.</span><span class="sxs-lookup"><span data-stu-id="9116e-132">.NET clients apps can use the BSON formatter with **HttpClient**.</span></span> <span data-ttu-id="9116e-133">Další informace o **HttpClient**, naleznete v tématu [volání webového rozhraní API z klienta .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="9116e-133">For more information about **HttpClient**, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="9116e-134">Následující kód odešle požadavek GET, která přijímá BSON a potom deserializuje datovou část BSON v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="9116e-134">The following code sends a GET request that accepts BSON, and then deserializes the BSON payload in the response.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

<span data-ttu-id="9116e-135">Chcete-li BSON vyžádat ze serveru, nastavte hlavičku Accept pro "application/bson":</span><span class="sxs-lookup"><span data-stu-id="9116e-135">To request BSON from the server, set the Accept header to "application/bson":</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

<span data-ttu-id="9116e-136">Chcete-li deserializovat tělo odpovědi, použijte **BsonMediaTypeFormatter**.</span><span class="sxs-lookup"><span data-stu-id="9116e-136">To deserialize the response body, use the **BsonMediaTypeFormatter**.</span></span> <span data-ttu-id="9116e-137">Tento formátovací modul není v kolekci výchozí formátování, takže budete muset zadat při čtení textu odpovědi:</span><span class="sxs-lookup"><span data-stu-id="9116e-137">This formatter is not in the default formatters collection, so you have to specify it when you read the response body:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

<span data-ttu-id="9116e-138">Následující příklad ukazuje, jak odeslat požadavek POST, který obsahuje BSON.</span><span class="sxs-lookup"><span data-stu-id="9116e-138">The next example shows how to send a POST request that contains BSON.</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

<span data-ttu-id="9116e-139">Velká část tento kód je stejný jako předchozí příklad.</span><span class="sxs-lookup"><span data-stu-id="9116e-139">Much of this code is the same as the previous example.</span></span> <span data-ttu-id="9116e-140">Ale **PostAsync** metody zadejte **BsonMediaTypeFormatter** jako formátovací modul:</span><span class="sxs-lookup"><span data-stu-id="9116e-140">But in the **PostAsync** method, specify **BsonMediaTypeFormatter** as the formatter:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a><span data-ttu-id="9116e-141">Serializace nejvyšší úrovně primitivní typy</span><span class="sxs-lookup"><span data-stu-id="9116e-141">Serializing Top-Level Primitive Types</span></span>

<span data-ttu-id="9116e-142">Každý dokument BSON je seznam dvojic klíč/hodnota. Specifikace formátu BSON nedefinuje syntaxe pro serializaci nezpracované hodnotu single, jako je například celé číslo nebo řetězec.</span><span class="sxs-lookup"><span data-stu-id="9116e-142">Every BSON document is a list of key/value pairs.The BSON specification does not define a syntax for serializing a single raw value, such as an integer or string.</span></span>

<span data-ttu-id="9116e-143">Chcete-li toto omezení **BsonMediaTypeFormatter** považuje za zvláštní případ primitivní typy.</span><span class="sxs-lookup"><span data-stu-id="9116e-143">To work around this limitation, the **BsonMediaTypeFormatter** treats primitive types as a special case.</span></span> <span data-ttu-id="9116e-144">Před serializací, převede hodnotu na dvojici klíč/hodnota s klíčem "Value".</span><span class="sxs-lookup"><span data-stu-id="9116e-144">Before serializing, it converts the value into a key/value pair with the key "Value".</span></span> <span data-ttu-id="9116e-145">Například předpokládejme, že vaše kontroleru rozhraní API vrátí celé číslo:</span><span class="sxs-lookup"><span data-stu-id="9116e-145">For example, suppose your API controller returns an integer:</span></span>

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

<span data-ttu-id="9116e-146">Před serializací, formátovací modul BSON převede na následující dvojici klíč/hodnota:</span><span class="sxs-lookup"><span data-stu-id="9116e-146">Before serializing, the BSON formatter converts this to the following key/value pair:</span></span>

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

<span data-ttu-id="9116e-147">Při deserializaci, formátovací modul převádí data zpět na původní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9116e-147">When you deserialize, the formatter converts the data back to the original value.</span></span> <span data-ttu-id="9116e-148">Však klienti, kteří používají jiný analyzátor BSON potřeba zpracovávat tento případ, pokud vaše webové rozhraní API vrátí nezpracované hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9116e-148">However, clients using a different BSON parser will need to handle this case, if your web API returns raw values.</span></span> <span data-ttu-id="9116e-149">Obecně platí měli byste zvážit, strukturovaná data, nikoli nezpracované hodnoty vrácením.</span><span class="sxs-lookup"><span data-stu-id="9116e-149">In general, you should consider returning structured data, rather than raw values.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9116e-150">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="9116e-150">Additional Resources</span></span>

[<span data-ttu-id="9116e-151">Ukázkové webové rozhraní API BSON</span><span class="sxs-lookup"><span data-stu-id="9116e-151">Web API BSON Sample</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[<span data-ttu-id="9116e-152">Formátovací moduly médií</span><span class="sxs-lookup"><span data-stu-id="9116e-152">Media Formatters</span></span>](media-formatters.md)
