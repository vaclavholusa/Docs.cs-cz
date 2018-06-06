---
title: Middleware komprese odpovědi pro ASP.NET Core
author: guardrex
description: Další informace o odpovědi komprese a jak používat Middleware komprese odpovědi v aplikacích ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: e970e74547f1f3efaf719c1f9e26918f34788005
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734624"
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="43d87-103">Middleware komprese odpovědi pro ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43d87-103">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="43d87-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="43d87-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="43d87-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="43d87-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="43d87-106">Šířka pásma sítě je omezená prostředků.</span><span class="sxs-lookup"><span data-stu-id="43d87-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="43d87-107">Rychlost reakce aplikace, je obvykle zmenšení velikosti odpovědi často výrazně zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="43d87-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="43d87-108">Jedním ze způsobů ke zmenšení velikosti datové části je kompresi odpovědí aplikace.</span><span class="sxs-lookup"><span data-stu-id="43d87-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="43d87-109">Kdy použít middlewaru odpovědi komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="43d87-110">Použijte technologie komprese odpovědi na serveru služby IIS, Apache nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="43d87-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="43d87-111">Výkon middleware pravděpodobně nebude odpovídat serverových modulů.</span><span class="sxs-lookup"><span data-stu-id="43d87-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="43d87-112">[Ovladač HTTP.sys serveru](xref:fundamentals/servers/httpsys) a [Kestrel](xref:fundamentals/servers/kestrel) aktuálně nenabízí podporu předdefinované komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="43d87-113">Middleware komprese odpovědi použijte, pokud jste:</span><span class="sxs-lookup"><span data-stu-id="43d87-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="43d87-114">Nelze použít následující technologie komprese na serveru:</span><span class="sxs-lookup"><span data-stu-id="43d87-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="43d87-115">Modul pro kompresi dynamického služby IIS</span><span class="sxs-lookup"><span data-stu-id="43d87-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="43d87-116">Apache mod_deflate modulu</span><span class="sxs-lookup"><span data-stu-id="43d87-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="43d87-117">Nginx komprese a dekomprese</span><span class="sxs-lookup"><span data-stu-id="43d87-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="43d87-118">Hostování přímo na:</span><span class="sxs-lookup"><span data-stu-id="43d87-118">Hosting directly on:</span></span>
  * <span data-ttu-id="43d87-119">[Ovladač HTTP.sys serveru](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="43d87-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="43d87-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="43d87-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="43d87-121">Komprese odpovědi</span><span class="sxs-lookup"><span data-stu-id="43d87-121">Response compression</span></span>

<span data-ttu-id="43d87-122">Obvykle všechny odpovědi komprimované nativně využívat výhod komprese odpovědi.</span><span class="sxs-lookup"><span data-stu-id="43d87-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="43d87-123">Odpovědi komprimované nativně obvykle zahrnují: šablon stylů CSS, JavaScript, HTML, XML a JSON.</span><span class="sxs-lookup"><span data-stu-id="43d87-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="43d87-124">Neměli komprimovat nativně komprimované prostředky, jako jsou například soubory PNG.</span><span class="sxs-lookup"><span data-stu-id="43d87-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="43d87-125">Když zkusíte další komprimovat nativně komprimované odpověď, jakékoli malé další snížení velikosti a přenosu čas pravděpodobně overshadowed o dobu, kterou trvalo zpracování komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="43d87-126">Nekomprimovat soubory menší než asi 1 150 000 bajtů (v závislosti na obsahu souboru a efektivitu komprese).</span><span class="sxs-lookup"><span data-stu-id="43d87-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="43d87-127">Režijní náklady komprimace malých souborů může vytvořit komprimovaný soubor větší než nekomprimovaného souboru.</span><span class="sxs-lookup"><span data-stu-id="43d87-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="43d87-128">Když klient může zpracovat komprimovaného obsahu, klient musí uvědomit serveru její možnosti `Accept-Encoding` hlavičky v požadavku.</span><span class="sxs-lookup"><span data-stu-id="43d87-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="43d87-129">Když server odešle komprimovaného obsahu, musí obsahovat informace v `Content-Encoding` záhlaví na tom, jak je zakódován zkomprimovanou odpověď.</span><span class="sxs-lookup"><span data-stu-id="43d87-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="43d87-130">V následující tabulce jsou uvedeny obsahu kódování označení nepodporuje middleware.</span><span class="sxs-lookup"><span data-stu-id="43d87-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="43d87-131">`Accept-Encoding` hodnoty hlavičky</span><span class="sxs-lookup"><span data-stu-id="43d87-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="43d87-132">Podporované middlewaru.</span><span class="sxs-lookup"><span data-stu-id="43d87-132">Middleware Supported</span></span> | <span data-ttu-id="43d87-133">Popis</span><span class="sxs-lookup"><span data-stu-id="43d87-133">Description</span></span>                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="43d87-134">Ne</span><span class="sxs-lookup"><span data-stu-id="43d87-134">No</span></span>                   | <span data-ttu-id="43d87-135">Formát Brotli komprimovaná Data</span><span class="sxs-lookup"><span data-stu-id="43d87-135">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="43d87-136">Ne</span><span class="sxs-lookup"><span data-stu-id="43d87-136">No</span></span>                   | <span data-ttu-id="43d87-137">Formát dat "komprimovat" systému UNIX</span><span class="sxs-lookup"><span data-stu-id="43d87-137">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="43d87-138">Ne</span><span class="sxs-lookup"><span data-stu-id="43d87-138">No</span></span>                   | <span data-ttu-id="43d87-139">"deflate" komprimovaná data uvnitř formát dat "zlib"</span><span class="sxs-lookup"><span data-stu-id="43d87-139">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="43d87-140">Ne</span><span class="sxs-lookup"><span data-stu-id="43d87-140">No</span></span>                   | <span data-ttu-id="43d87-141">Výměnu efektivní XML W3C</span><span class="sxs-lookup"><span data-stu-id="43d87-141">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="43d87-142">Ano (výchozí)</span><span class="sxs-lookup"><span data-stu-id="43d87-142">Yes (default)</span></span>        | <span data-ttu-id="43d87-143">Formát souboru GZIP</span><span class="sxs-lookup"><span data-stu-id="43d87-143">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="43d87-144">Ano</span><span class="sxs-lookup"><span data-stu-id="43d87-144">Yes</span></span>                  | <span data-ttu-id="43d87-145">"Bez kódování" identifikátor: odpověď nesmí být zakódován.</span><span class="sxs-lookup"><span data-stu-id="43d87-145">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="43d87-146">Ne</span><span class="sxs-lookup"><span data-stu-id="43d87-146">No</span></span>                   | <span data-ttu-id="43d87-147">Formát přenosu sítě pro Java archivy</span><span class="sxs-lookup"><span data-stu-id="43d87-147">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="43d87-148">Ano</span><span class="sxs-lookup"><span data-stu-id="43d87-148">Yes</span></span>                  | <span data-ttu-id="43d87-149">Žádný dostupný obsah kódování není explicitně požadovaný</span><span class="sxs-lookup"><span data-stu-id="43d87-149">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="43d87-150">Další informace najdete v tématu [IANA oficiální obsahu kódování seznamu](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="43d87-150">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="43d87-151">Middleware umožňuje přidat další komprese zprostředkovatele pro vlastní `Accept-Encoding` hodnoty hlavičky.</span><span class="sxs-lookup"><span data-stu-id="43d87-151">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="43d87-152">Další informace najdete v tématu [Vlastní zprostředkovatelé](#custom-providers) níže.</span><span class="sxs-lookup"><span data-stu-id="43d87-152">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="43d87-153">Middleware je schopný reagovat na hodnotu kvality (qvalue, `q`) vážení při odeslání klient k určení priority schémat komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-153">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="43d87-154">Další informace najdete v tématu [RFC 7231: přijměte kódování](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="43d87-154">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="43d87-155">Algoritmy komprese podléhají kompromis mezi rychlostí komprese a efektivitu komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-155">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="43d87-156">*Efektivita* v tomto kontextu odkazuje na velikost výstupu po kompresi.</span><span class="sxs-lookup"><span data-stu-id="43d87-156">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="43d87-157">Nejmenší velikost se dosahuje nejvíc *optimální* komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-157">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="43d87-158">Hlavičky účastnící se požaduje, odesílání, ukládání do mezipaměti a přijímání komprimovaného obsahu jsou popsány v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="43d87-158">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="43d87-159">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="43d87-159">Header</span></span>             | <span data-ttu-id="43d87-160">Role</span><span class="sxs-lookup"><span data-stu-id="43d87-160">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="43d87-161">Odeslaných z klienta na server k označení obsahu, kódování schémata přijatelné pro klienta.</span><span class="sxs-lookup"><span data-stu-id="43d87-161">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="43d87-162">Odeslaných ze serveru do klienta k označení, kódování obsahu v datové části.</span><span class="sxs-lookup"><span data-stu-id="43d87-162">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="43d87-163">Když dojde k komprese, `Content-Length` záhlaví je odebrán, od změny obsahu textu při zkomprimovanou odpověď.</span><span class="sxs-lookup"><span data-stu-id="43d87-163">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="43d87-164">Když dojde k komprese, `Content-MD5` záhlaví je odebrán, protože došlo ke změně textu obsahu a hodnota hash je již neplatný.</span><span class="sxs-lookup"><span data-stu-id="43d87-164">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="43d87-165">Určuje typ MIME obsahu.</span><span class="sxs-lookup"><span data-stu-id="43d87-165">Specifies the MIME type of the content.</span></span> <span data-ttu-id="43d87-166">Každé odpovědi by měl určovat jeho `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="43d87-166">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="43d87-167">Middleware ověří tuto hodnotu, která určí, pokud odpověď by měla být zkomprimuje.</span><span class="sxs-lookup"><span data-stu-id="43d87-167">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="43d87-168">Middleware určuje sadu [výchozí typy MIME](#mime-types) , může být kódován, ale můžete nahradit nebo přidat typy MIME.</span><span class="sxs-lookup"><span data-stu-id="43d87-168">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="43d87-169">Při odeslání serveru s hodnotou `Accept-Encoding` pro klienty a proxy servery, `Vary` Hlavička uvádí, klienta nebo proxy server, který by měl mezipaměti (lišit) na základě odpovědí na základě hodnoty `Accept-Encoding` hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="43d87-169">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="43d87-170">Výsledek vrácení obsahu pomocí `Vary: Accept-Encoding` záhlaví je, že oba komprimované a jsou v nekomprimované odpovědi samostatně ukládat do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="43d87-170">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="43d87-171">Můžete prozkoumat funkce middlewaru komprese odpovědi s [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="43d87-171">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="43d87-172">Ukázka předvádí:</span><span class="sxs-lookup"><span data-stu-id="43d87-172">The sample illustrates:</span></span>

* <span data-ttu-id="43d87-173">Komprese odpovědí aplikace pomocí gzip a poskytovatelé vlastní komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-173">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="43d87-174">Jak přidat typ MIME do seznamu výchozích typů standardu MIME pro kompresi.</span><span class="sxs-lookup"><span data-stu-id="43d87-174">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="43d87-175">Balíček</span><span class="sxs-lookup"><span data-stu-id="43d87-175">Package</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="43d87-176">Zahrnout middleware projekt, přidejte odkaz na [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="43d87-176">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span> <span data-ttu-id="43d87-177">Tato funkce je k dispozici pro aplikace, které cílí ASP.NET Core 1.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="43d87-177">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="43d87-178">Zahrnout middleware projekt, přidejte odkaz na [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) balíček nebo použít [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="43d87-178">To include the middleware in your project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="43d87-179">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="43d87-179">Configuration</span></span>

<span data-ttu-id="43d87-180">Následující kód ukazuje, jak povolit kompresi gzip výchozí a pro typy MIME výchozí Middleware komprese odpovědi.</span><span class="sxs-lookup"><span data-stu-id="43d87-180">The following code shows how to enable the Response Compression Middleware with the default gzip compression and for default MIME types.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

> [!NOTE]
> <span data-ttu-id="43d87-181">Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), nebo [Postman](https://www.getpostman.com/) nastavit `Accept-Encoding` hlavička požadavku a prostudovali hlavičky odpovědi, velikost a text.</span><span class="sxs-lookup"><span data-stu-id="43d87-181">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="43d87-182">Odeslat žádost o ukázková aplikace bez `Accept-Encoding` záhlaví a zjistíte, že je odpověď na nekomprimované.</span><span class="sxs-lookup"><span data-stu-id="43d87-182">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="43d87-183">`Content-Encoding` a `Vary` hlavičky nejsou k dispozici na odpověď.</span><span class="sxs-lookup"><span data-stu-id="43d87-183">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Zobrazuje výsledek požadavku bez hlavičky Accept-Encoding okno aplikaci Fiddler.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="43d87-186">Odeslat žádost o vzorovou aplikaci s `Accept-Encoding: gzip` záhlaví a zjistíte, že zkomprimovanou odpověď.</span><span class="sxs-lookup"><span data-stu-id="43d87-186">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="43d87-187">`Content-Encoding` a `Vary` hlavičky se nacházejí na odpověď.</span><span class="sxs-lookup"><span data-stu-id="43d87-187">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Výsledek požadavku s hlavičky Accept-Encoding, a hodnota gzip okno aplikaci Fiddler.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="43d87-191">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="43d87-191">Providers</span></span>

### <a name="gzipcompressionprovider"></a><span data-ttu-id="43d87-192">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="43d87-192">GzipCompressionProvider</span></span>

<span data-ttu-id="43d87-193">Použití [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) kompresi odpovědí s gzip.</span><span class="sxs-lookup"><span data-stu-id="43d87-193">Use the [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) to compress responses with gzip.</span></span> <span data-ttu-id="43d87-194">Toto je výchozí zprostředkovatel komprese-li zadán žádný.</span><span class="sxs-lookup"><span data-stu-id="43d87-194">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="43d87-195">Komprese můžete nastavit úroveň s [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span><span class="sxs-lookup"><span data-stu-id="43d87-195">You can set the compression level with the [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).</span></span>

<span data-ttu-id="43d87-196">Výchozí zprostředkovatel kompresi gzip nejrychlejší úroveň komprese ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), který nemusí vracet nejúčinnější komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-196">The gzip compression provider defaults to the fastest compression level ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="43d87-197">Pokud se požaduje nejúčinnější komprese, můžete nakonfigurovat middleware pro optimální komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-197">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="43d87-198">Úroveň komprese</span><span class="sxs-lookup"><span data-stu-id="43d87-198">Compression Level</span></span> | <span data-ttu-id="43d87-199">Popis</span><span class="sxs-lookup"><span data-stu-id="43d87-199">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="43d87-200">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="43d87-200">CompressionLevel.Fastest</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="43d87-201">Komprese co nejrychleji provést i v případě, že výsledný výstup není komprimována optimálně.</span><span class="sxs-lookup"><span data-stu-id="43d87-201">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="43d87-202">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="43d87-202">CompressionLevel.NoCompression</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="43d87-203">Je třeba provést bez komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-203">No compression should be performed.</span></span> |
| [<span data-ttu-id="43d87-204">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="43d87-204">CompressionLevel.Optimal</span></span>](/dotnet/api/system.io.compression.compressionlevel) | <span data-ttu-id="43d87-205">Odpovědi musí být optimálně komprimován, i v případě, že komprese trvá déle.</span><span class="sxs-lookup"><span data-stu-id="43d87-205">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43d87-206">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="43d87-206">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43d87-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43d87-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a><span data-ttu-id="43d87-208">typy MIME</span><span class="sxs-lookup"><span data-stu-id="43d87-208">MIME types</span></span>

<span data-ttu-id="43d87-209">Middleware určuje sadu výchozích typů standardu MIME pro kompresi:</span><span class="sxs-lookup"><span data-stu-id="43d87-209">The middleware specifies a default set of MIME types for compression:</span></span>

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="43d87-210">Můžete nahradit nebo připojit typy MIME s možnosti middlewaru komprese odpovědi.</span><span class="sxs-lookup"><span data-stu-id="43d87-210">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="43d87-211">Všimněte si, že zástupný znak standardu MIME typy, jako například `text/*` nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="43d87-211">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="43d87-212">Ukázková aplikace přidá typ MIME pro `image/svg+xml` komprimuje a slouží ASP.NET Core banner image (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="43d87-212">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43d87-213">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="43d87-213">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43d87-214">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43d87-214">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a><span data-ttu-id="43d87-215">Vlastní zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="43d87-215">Custom providers</span></span>

<span data-ttu-id="43d87-216">Můžete vytvořit vlastní komprese implementace s [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span><span class="sxs-lookup"><span data-stu-id="43d87-216">You can create custom compression implementations with [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider).</span></span> <span data-ttu-id="43d87-217">[EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) reprezentuje obsah, který tato kódování `ICompressionProvider` vytváří.</span><span class="sxs-lookup"><span data-stu-id="43d87-217">The [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="43d87-218">Middleware používá tuto informaci k vyberte poskytovatele správy na základě zadané v seznamu `Accept-Encoding` hlavičky žádosti.</span><span class="sxs-lookup"><span data-stu-id="43d87-218">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="43d87-219">Použití ukázkové aplikace, klient odešle žádost s `Accept-Encoding: mycustomcompression` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="43d87-219">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="43d87-220">Middleware použije implementace vlastní komprese a vrátí odpověď se `Content-Encoding: mycustomcompression` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="43d87-220">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="43d87-221">Klient musí umět dekomprimovat vlastní kódování v pořadí pro implementaci vlastní komprese pracovat.</span><span class="sxs-lookup"><span data-stu-id="43d87-221">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43d87-222">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="43d87-222">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43d87-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43d87-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="43d87-224">Odeslat žádost o vzorovou aplikaci s `Accept-Encoding: mycustomcompression` záhlaví a sledovat hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="43d87-224">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="43d87-225">`Vary` a `Content-Encoding` hlavičky se nacházejí na odpověď.</span><span class="sxs-lookup"><span data-stu-id="43d87-225">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="43d87-226">Text odpovědi (není vidět) není komprimována ve vzorku.</span><span class="sxs-lookup"><span data-stu-id="43d87-226">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="43d87-227">Není k dispozici implementace komprese ve `CustomCompressionProvider` třída vzorku.</span><span class="sxs-lookup"><span data-stu-id="43d87-227">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="43d87-228">Ukázka však ukazuje, kde by implementovat algoritmus komprese.</span><span class="sxs-lookup"><span data-stu-id="43d87-228">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Výsledek požadavku s hlavičky Accept-Encoding, a hodnota mycustomcompression okno aplikaci Fiddler.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="43d87-231">Komprese s zabezpečený protokol</span><span class="sxs-lookup"><span data-stu-id="43d87-231">Compression with secure protocol</span></span>

<span data-ttu-id="43d87-232">Komprimované odpovědi prostřednictvím zabezpečeného připojení se dá řídit pomocí `EnableForHttps` možnost, která je ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="43d87-232">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="43d87-233">Komprese pomocí dynamicky generovaném stránky může způsobit problémy se zabezpečením, jako [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) a [porušení](https://wikipedia.org/wiki/BREACH_(security_exploit)) útoky.</span><span class="sxs-lookup"><span data-stu-id="43d87-233">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="43d87-234">Přidání měnit hlavičky</span><span class="sxs-lookup"><span data-stu-id="43d87-234">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="43d87-235">Při kompresi odpovědí na základě `Accept-Encoding` záhlaví, existují potenciálně více komprimované verze odpovědi a nekomprimované verze.</span><span class="sxs-lookup"><span data-stu-id="43d87-235">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="43d87-236">Chcete-li pokyn mezipaměti klienta a serveru proxy, několik verzí existují a by měla být uložena, `Vary` záhlaví se přidá s `Accept-Encoding` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="43d87-236">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="43d87-237">V technologii ASP.NET Core 2.0 nebo novější, přidá middleware `Vary` záhlaví automaticky, když zkomprimovanou odpověď.</span><span class="sxs-lookup"><span data-stu-id="43d87-237">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="43d87-238">Při kompresi odpovědí na základě `Accept-Encoding` záhlaví, existují potenciálně více komprimované verze odpovědi a nekomprimované verze.</span><span class="sxs-lookup"><span data-stu-id="43d87-238">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="43d87-239">Chcete-li pokyn mezipaměti klienta a serveru proxy, několik verzí existují a by měla být uložena, `Vary` záhlaví se přidá s `Accept-Encoding` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="43d87-239">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="43d87-240">V ASP.NET Core 1.x, přidání `Vary` hlavičky odpovědi dosahuje ručně:</span><span class="sxs-lookup"><span data-stu-id="43d87-240">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

<span data-ttu-id="43d87-241">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="43d87-241">[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="43d87-242">Middleware problém při za Nginx reverzní proxy server</span><span class="sxs-lookup"><span data-stu-id="43d87-242">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="43d87-243">Pokud je požadavek směrovány přes proxy server pomocí Nginx, `Accept-Encoding` záhlaví se odebere.</span><span class="sxs-lookup"><span data-stu-id="43d87-243">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="43d87-244">Middleware zabrání kompresi odpovědi.</span><span class="sxs-lookup"><span data-stu-id="43d87-244">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="43d87-245">Další informace najdete v tématu [NGINX: komprese a dekomprese](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="43d87-245">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="43d87-246">Tento problém je sledovanými [zjistěte průchozí komprese Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="43d87-246">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="43d87-247">Práce s dynamické komprese služby IIS</span><span class="sxs-lookup"><span data-stu-id="43d87-247">Working with IIS dynamic compression</span></span>

<span data-ttu-id="43d87-248">Pokud máte aktivní IIS dynamické komprese modul nakonfigurována na úrovni serveru, který chcete zakázat pro aplikaci, můžete tak učinit s doplňkem k vaší *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="43d87-248">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="43d87-249">Další informace najdete v tématu [moduly IIS zakázání](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="43d87-249">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="43d87-250">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="43d87-250">Troubleshooting</span></span>

<span data-ttu-id="43d87-251">Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), nebo [Postman](https://www.getpostman.com/), které umožňují nastavit `Accept-Encoding` hlavička požadavku a prostudovali hlavičky odpovědi, velikost a text.</span><span class="sxs-lookup"><span data-stu-id="43d87-251">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="43d87-252">Middleware komprese odpovědi komprimaci odpovědi, které splňují následující podmínky:</span><span class="sxs-lookup"><span data-stu-id="43d87-252">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="43d87-253">`Accept-Encoding` Záhlaví nachází s hodnotou `gzip`, `*`, nebo vlastní kódování, který odpovídá komprese vlastní zprostředkovatele, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="43d87-253">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="43d87-254">Hodnota nesmí být `identity` nebo obsahovat hodnotu kvality (qvalue, `q`) nastavení 0 (nula).</span><span class="sxs-lookup"><span data-stu-id="43d87-254">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="43d87-255">Typ MIME (`Content-Type`) musí být nastavená a nakonfigurovaná na typ MIME se musí shodovat [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span><span class="sxs-lookup"><span data-stu-id="43d87-255">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).</span></span>
* <span data-ttu-id="43d87-256">Nesmí zahrnovat požadavek `Content-Range` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="43d87-256">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="43d87-257">Pokud zabezpečeného protokolu (https) je nakonfigurován v možnosti middlewaru komprese odpovědi, musíte použít požadavek nezabezpečené protokol (http).</span><span class="sxs-lookup"><span data-stu-id="43d87-257">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="43d87-258">*Všimněte si nebezpečí [popsané výše](#compression-with-secure-protocol) při povolování zabezpečené komprese obsahu.*</span><span class="sxs-lookup"><span data-stu-id="43d87-258">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43d87-259">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="43d87-259">Additional resources</span></span>

* [<span data-ttu-id="43d87-260">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="43d87-260">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="43d87-261">Middleware</span><span class="sxs-lookup"><span data-stu-id="43d87-261">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="43d87-262">Mozilla Developer Network: Přijměte kódování</span><span class="sxs-lookup"><span data-stu-id="43d87-262">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="43d87-263">RFC 7231 části 3.1.2.1: Obsahu Codings</span><span class="sxs-lookup"><span data-stu-id="43d87-263">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="43d87-264">RFC 7230 oddílu 4.2.3: Kódování Gzip</span><span class="sxs-lookup"><span data-stu-id="43d87-264">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="43d87-265">Verze specifikaci formátu souboru GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="43d87-265">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
