---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: "Volání webového rozhraní API z klienta .NET (C#) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8156bd1c7cfc111a6a121a89d845ca284ee1b7af
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="d9cf9-102">Volání webového rozhraní API z klienta .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="d9cf9-102">Call a Web API From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="d9cf9-103">podle [Karel Wasson](https://github.com/MikeWasson) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d9cf9-103">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="d9cf9-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="d9cf9-104">Download Completed Project</span></span>](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

<span data-ttu-id="d9cf9-105">Tento kurz ukazuje způsob volání webového rozhraní API z aplikace .NET, pomocí [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="d9cf9-105">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="d9cf9-106">V tomto kurzu je zapsán klientskou aplikaci, která využívá následující webové rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-106">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="d9cf9-107">Akce</span><span class="sxs-lookup"><span data-stu-id="d9cf9-107">Action</span></span> | <span data-ttu-id="d9cf9-108">Metoda HTTP</span><span class="sxs-lookup"><span data-stu-id="d9cf9-108">HTTP method</span></span> | <span data-ttu-id="d9cf9-109">Relativní identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="d9cf9-109">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d9cf9-110">Získání ID produktu</span><span class="sxs-lookup"><span data-stu-id="d9cf9-110">Get a product by ID</span></span> | <span data-ttu-id="d9cf9-111">GET</span><span class="sxs-lookup"><span data-stu-id="d9cf9-111">GET</span></span> | <span data-ttu-id="d9cf9-112">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d9cf9-112">/api/products/*id*</span></span> |
| <span data-ttu-id="d9cf9-113">Vytvoření nového produktu</span><span class="sxs-lookup"><span data-stu-id="d9cf9-113">Create a new product</span></span> | <span data-ttu-id="d9cf9-114">POST</span><span class="sxs-lookup"><span data-stu-id="d9cf9-114">POST</span></span> | <span data-ttu-id="d9cf9-115">/ api/produkty</span><span class="sxs-lookup"><span data-stu-id="d9cf9-115">/api/products</span></span> |
| <span data-ttu-id="d9cf9-116">Aktualizace produktu</span><span class="sxs-lookup"><span data-stu-id="d9cf9-116">Update a product</span></span> | <span data-ttu-id="d9cf9-117">PUT</span><span class="sxs-lookup"><span data-stu-id="d9cf9-117">PUT</span></span> | <span data-ttu-id="d9cf9-118">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d9cf9-118">/api/products/*id*</span></span> |
| <span data-ttu-id="d9cf9-119">Odstranit produktu</span><span class="sxs-lookup"><span data-stu-id="d9cf9-119">Delete a product</span></span> | <span data-ttu-id="d9cf9-120">DELETE</span><span class="sxs-lookup"><span data-stu-id="d9cf9-120">DELETE</span></span> | <span data-ttu-id="d9cf9-121">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d9cf9-121">/api/products/*id*</span></span> |

<span data-ttu-id="d9cf9-122">Pokud chcete dozvědět, jak implementovat toto rozhraní API s rozhraním ASP.NET Web API, přečtěte si téma [vytváření webového rozhraní API této operace CRUD podporuje](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="d9cf9-122">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="d9cf9-123">Pro jednoduchost klientská aplikace v tomto kurzu je konzolová aplikace z Windows.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-123">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="d9cf9-124">**HttpClient** je také podporována pro aplikace Windows Phone a Windows Store.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-124">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="d9cf9-125">Další informace najdete v tématu [psaní webové rozhraní API klienta kódu pro více platforem, pomocí přenosné knihovny](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="d9cf9-125">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="d9cf9-126">Vytvoření konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="d9cf9-126">Create the Console Application</span></span>

<span data-ttu-id="d9cf9-127">V sadě Visual Studio vytvořte novou konzolovou aplikaci systému Windows s názvem **HttpClientSample** a vložte následující kód:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-127">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="d9cf9-128">Předchozí kód je kompletní klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-128">The preceding code is the complete client app.</span></span>

<span data-ttu-id="d9cf9-129">`RunAsync`Spustí a bloky až do dokončení.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-129">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="d9cf9-130">Většina **HttpClient** metody jsou asynchronní, protože fungují v/v sítě.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-130">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="d9cf9-131">Všechny asynchronních úloh se provádějí v `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-131">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="d9cf9-132">Za normálních okolností aplikace neblokuje hlavního vlákna, ale tato aplikace nebude povolovat interakce.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-132">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="d9cf9-133">Nainstalujte knihovny webové rozhraní API klienta</span><span class="sxs-lookup"><span data-stu-id="d9cf9-133">Install the Web API Client Libraries</span></span>

<span data-ttu-id="d9cf9-134">Použití Správce balíčků NuGet nainstalujte balíček knihovny klienta webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-134">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="d9cf9-135">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-135">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="d9cf9-136">V balíček správce konzoly (pomocí PMC), zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-136">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="d9cf9-137">Předchozí příkaz přidá následující balíčky NuGet do projektu:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-137">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="d9cf9-138">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="d9cf9-138">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="d9cf9-139">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="d9cf9-139">Newtonsoft.Json</span></span>

<span data-ttu-id="d9cf9-140">Json.NET je oblíbených rozhraní vysoce výkonné JSON pro rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-140">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="d9cf9-141">Přidejte třídu modelu</span><span class="sxs-lookup"><span data-stu-id="d9cf9-141">Add a Model Class</span></span>

<span data-ttu-id="d9cf9-142">Zkontrolujte `Product` třídy:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-142">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="d9cf9-143">Tato třída neodpovídá modelu dat používá webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-143">This class matches the data model used by the web API.</span></span> <span data-ttu-id="d9cf9-144">Aplikace můžete použít **HttpClient** ke čtení `Product` instance z odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-144">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="d9cf9-145">Aplikace nemá psaní jakéhokoli kódu deserializace.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-145">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="d9cf9-146">Vytvoření a inicializace HttpClient</span><span class="sxs-lookup"><span data-stu-id="d9cf9-146">Create and Initialize HttpClient</span></span>

<span data-ttu-id="d9cf9-147">Zkontrolujte statických **HttpClient** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-147">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="d9cf9-148">**HttpClient** je určená pro vytvoření instance jednou a znovu po celou dobu životnosti aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-148">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="d9cf9-149">Může mít za následek následující podmínky **SocketException** chyby:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-149">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="d9cf9-150">Vytvoření nové **HttpClient** instance pro každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-150">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="d9cf9-151">Server v případě velkého zatížení.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-151">Server under heavy load.</span></span>

<span data-ttu-id="d9cf9-152">Vytvoření nové **HttpClient** instance pro každý požadavek může vyčerpat dostupných soketů.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-152">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="d9cf9-153">Následující kód inicializuje **HttpClient** instance:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-153">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="d9cf9-154">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-154">The preceding code:</span></span>

* <span data-ttu-id="d9cf9-155">Nastaví základní identifikátor URI pro požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-155">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="d9cf9-156">Změňte číslo portu na port použitý při aplikace serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-156">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="d9cf9-157">Aplikace nebude fungovat, pokud se používá port pro aplikace serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-157">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="d9cf9-158">Nastaví hlavičku Accept pro "application/json".</span><span class="sxs-lookup"><span data-stu-id="d9cf9-158">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="d9cf9-159">Nastavení této hlavičky informuje server k odesílání dat ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-159">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="d9cf9-160">Odeslat požadavek GET pro načtení prostředku</span><span class="sxs-lookup"><span data-stu-id="d9cf9-160">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="d9cf9-161">Následující kód odešle požadavek GET pro produkt:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-161">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="d9cf9-162">**GetAsync** metoda odešle požadavek HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-162">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="d9cf9-163">Po metodě dokončení vrátí **objekt HttpResponseMessage** , který obsahuje odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-163">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="d9cf9-164">Pokud stavový kód v odpovědi kód úspěch, text odpovědi obsahuje reprezentace JSON produktu.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-164">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="d9cf9-165">Volání **ReadAsAsync** k datové části JSON k deserializaci `Product` instance.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-165">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="d9cf9-166">**ReadAsAsync** metoda je asynchronní, protože text odpovědi lze libovolně velké.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-166">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="d9cf9-167">**HttpClient** nevyvolá výjimku, pokud odpověď HTTP, která obsahuje kód chyby.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-167">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="d9cf9-168">Místo toho **IsSuccessStatusCode** vlastnost je **false** Pokud je stav chybový kód.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-168">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="d9cf9-169">Pokud dáváte přednost zacházet s kódy chyb HTTP jako výjimky, volání [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) na objekt odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-169">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="d9cf9-170">`EnsureSuccessStatusCode`vyvolá výjimku, pokud kód stavu spadá mimo rozsah 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-170">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="d9cf9-171">Všimněte si, že **HttpClient** může vyvolat výjimky z jiných důvodů &mdash; například, pokud vyprší časový limit žádosti.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-171">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="d9cf9-172">Formátovací moduly typu média k deserializaci</span><span class="sxs-lookup"><span data-stu-id="d9cf9-172">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="d9cf9-173">Když **ReadAsAsync** je volána bez parametrů, používá výchozí sadu *formátovací moduly pro média* ke čtení textu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-173">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="d9cf9-174">Výchozí formátování podporovat JSON, XML a data formuláře kódovaná adresy url.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-174">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="d9cf9-175">Místo použití výchozí formátování, můžete zadat seznam formátovací moduly, které **ReadAsAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-175">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="d9cf9-176">Použití seznam formátovacích modulů je užitečné, pokud máte vlastní typ média formátování:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-176">Using a a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="d9cf9-177">Další informace najdete v tématu [formátovací moduly pro média ve webovém rozhraní API 2 ASP.NET](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="d9cf9-177">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="d9cf9-178">Odesílání požadavku POST k vytvoření prostředku</span><span class="sxs-lookup"><span data-stu-id="d9cf9-178">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="d9cf9-179">Odešle požadavek POST, který obsahuje následující kód `Product` instance ve formátu JSON:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-179">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="d9cf9-180">**PostAsJsonAsync** metoda:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-180">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="d9cf9-181">Serializuje objekt do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-181">Serializes an object to JSON.</span></span>
* <span data-ttu-id="d9cf9-182">Odešle datové části JSON v požadavku POST.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-182">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="d9cf9-183">Pokud neproběhne:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-183">If the request succeeds:</span></span>

* <span data-ttu-id="d9cf9-184">Měla by vrátit odpověď 201 (vytvořeno).</span><span class="sxs-lookup"><span data-stu-id="d9cf9-184">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="d9cf9-185">Odpověď by měla obsahovat adresu URL vytvořené prostředky hlavička umístění.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-185">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="d9cf9-186">Odesílání PUT žádost o aktualizaci prostředku</span><span class="sxs-lookup"><span data-stu-id="d9cf9-186">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="d9cf9-187">Následující kód odešle požadavek PUT pro aktualizaci produktu:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-187">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="d9cf9-188">**PutAsJsonAsync** metoda se dá použít jako **PostAsJsonAsync**kromě toho, že odešle požadavek PUT místo POST.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-188">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="d9cf9-189">Odesílání požadavku na odstranění odstranění prostředku</span><span class="sxs-lookup"><span data-stu-id="d9cf9-189">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="d9cf9-190">Následující kód odešle požadavek DELETE k odstranění produktu:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-190">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="d9cf9-191">Žádost o odstranění jako GET, nemá textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-191">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="d9cf9-192">Nemusíte zadejte ve formátu JSON nebo XML s odstranit.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-192">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="d9cf9-193">Testování vzorku</span><span class="sxs-lookup"><span data-stu-id="d9cf9-193">Test the sample</span></span>

<span data-ttu-id="d9cf9-194">K testování aplikace klienta:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-194">To test the client app:</span></span>

1. <span data-ttu-id="d9cf9-195">[Stáhněte si](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) a spuštění aplikace serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-195">[Download](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="d9cf9-196">[Pokyny ke stahování](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d9cf9-196">[Download instructions](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> <span data-ttu-id="d9cf9-197">Ověřte, zda že je funkční aplikace serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-197">Verify the server app is working.</span></span> <span data-ttu-id="d9cf9-198">Pro exaxmple `http://localhost:64195/api/products` by měla vrátit seznam produktů.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-198">For exaxmple, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="d9cf9-199">Nastaví základní identifikátor URI pro požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-199">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="d9cf9-200">Změňte číslo portu na port použitý při aplikace serveru.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-200">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="d9cf9-201">Spusťte klientskou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d9cf9-201">Run the client app.</span></span> <span data-ttu-id="d9cf9-202">Vytváří následující výstup:</span><span class="sxs-lookup"><span data-stu-id="d9cf9-202">The following output is produced:</span></span>

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
