---
title: Kompresi odpovědí v ASP.NET Core
author: guardrex
description: Další informace o kompresi odpovědí a jak používat Middleware pro kompresi odpovědí v aplikacích ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: performance/response-compression
ms.openlocfilehash: 3a01c2d572c0026944347f736f9658a7872e6c35
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028281"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="82287-103">Kompresi odpovědí v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="82287-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="82287-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="82287-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="82287-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="82287-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="82287-106">Šířka pásma sítě je omezená prostředků.</span><span class="sxs-lookup"><span data-stu-id="82287-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="82287-107">Na odezvu aplikace, je obvykle nezmenšit velikost této odpovědi často výrazně zvyšuje.</span><span class="sxs-lookup"><span data-stu-id="82287-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="82287-108">Jedním ze způsobů ke zmenšení velikosti datové části je kompresi odpovědí vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="82287-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="82287-109">Kdy použít Middleware pro kompresi odpovědí</span><span class="sxs-lookup"><span data-stu-id="82287-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="82287-110">Použití technologie komprese odpovědi na serveru IIS, Apache nebo Nginx.</span><span class="sxs-lookup"><span data-stu-id="82287-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="82287-111">Výkon middleware pravděpodobně nebude odpovídat moduly serveru.</span><span class="sxs-lookup"><span data-stu-id="82287-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="82287-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) a [Kestrel](xref:fundamentals/servers/kestrel) aktuálně nenabízí podporu integrovanou komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="82287-113">Middleware pro kompresi odpovědí použijte, když jste:</span><span class="sxs-lookup"><span data-stu-id="82287-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="82287-114">Nelze použít následující technologie serverových komprese:</span><span class="sxs-lookup"><span data-stu-id="82287-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="82287-115">Modul dynamická komprese služby IIS</span><span class="sxs-lookup"><span data-stu-id="82287-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="82287-116">Apache mod_deflate modulu</span><span class="sxs-lookup"><span data-stu-id="82287-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="82287-117">Server Nginx komprese a dekomprese</span><span class="sxs-lookup"><span data-stu-id="82287-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="82287-118">Hostování přímo na:</span><span class="sxs-lookup"><span data-stu-id="82287-118">Hosting directly on:</span></span>
  * <span data-ttu-id="82287-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (dříve se označovaly jako [WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="82287-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="82287-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="82287-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="82287-121">Kompresi odpovědí</span><span class="sxs-lookup"><span data-stu-id="82287-121">Response compression</span></span>

<span data-ttu-id="82287-122">Obvykle žádnou odpověď komprimované nativně využívat kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="82287-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="82287-123">Odpovědí se komprimované nativně obvykle zahrnují: šablony stylů CSS, JavaScript, HTML, XML a JSON.</span><span class="sxs-lookup"><span data-stu-id="82287-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="82287-124">Nativně komprimované prostředky, jako jsou například soubory PNG by neměly komprimovat.</span><span class="sxs-lookup"><span data-stu-id="82287-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="82287-125">Pokud se pokusíte další komprimovat nativně zkomprimovanou odpověď, všechny malé Další velikosti a přenos zkrácení času nutného pravděpodobně overshadowed v době, jakou trvalo zpracování komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="82287-126">Nekomprimovat soubory menší než přibližně 150 – 1 000 bajtů (v závislosti na obsahu souboru a efektivity komprese).</span><span class="sxs-lookup"><span data-stu-id="82287-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="82287-127">Režie komprimace malých souborů může vytvořit komprimovaný soubor větší než nekomprimovaného souboru.</span><span class="sxs-lookup"><span data-stu-id="82287-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="82287-128">Když klient může zpracovat komprimovaného obsahu, klient informuje server její možnosti odesláním `Accept-Encoding` záhlaví s požadavkem.</span><span class="sxs-lookup"><span data-stu-id="82287-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="82287-129">Když server odešle komprimovaného obsahu, musí obsahovat informace `Content-Encoding` záhlaví na tom, jak je zakódován zkomprimovanou odpověď.</span><span class="sxs-lookup"><span data-stu-id="82287-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="82287-130">V následující tabulce jsou uvedeny obsahu kódování označení podporované middlewarem.</span><span class="sxs-lookup"><span data-stu-id="82287-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="82287-131">`Accept-Encoding` hodnoty hlavičky</span><span class="sxs-lookup"><span data-stu-id="82287-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="82287-132">Middleware podporována</span><span class="sxs-lookup"><span data-stu-id="82287-132">Middleware Supported</span></span> | <span data-ttu-id="82287-133">Popis</span><span class="sxs-lookup"><span data-stu-id="82287-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="82287-134">Ano (výchozí)</span><span class="sxs-lookup"><span data-stu-id="82287-134">Yes (default)</span></span>        | [<span data-ttu-id="82287-135">Formát komprimovaných dat Brotli</span><span class="sxs-lookup"><span data-stu-id="82287-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="82287-136">Ne</span><span class="sxs-lookup"><span data-stu-id="82287-136">No</span></span>                   | [<span data-ttu-id="82287-137">Formát komprimovaných dat DEFLATE</span><span class="sxs-lookup"><span data-stu-id="82287-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="82287-138">Ne</span><span class="sxs-lookup"><span data-stu-id="82287-138">No</span></span>                   | [<span data-ttu-id="82287-139">W3C XML efektivní výměny</span><span class="sxs-lookup"><span data-stu-id="82287-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="82287-140">Ano</span><span class="sxs-lookup"><span data-stu-id="82287-140">Yes</span></span>                  | [<span data-ttu-id="82287-141">Formát souborů GZIP</span><span class="sxs-lookup"><span data-stu-id="82287-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="82287-142">Ano</span><span class="sxs-lookup"><span data-stu-id="82287-142">Yes</span></span>                  | <span data-ttu-id="82287-143">"Bez kódování" identifikátor: odpověď nesmí být zakódován.</span><span class="sxs-lookup"><span data-stu-id="82287-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="82287-144">Ne</span><span class="sxs-lookup"><span data-stu-id="82287-144">No</span></span>                   | [<span data-ttu-id="82287-145">Formát přenosu sítě pro archivy Java</span><span class="sxs-lookup"><span data-stu-id="82287-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="82287-146">Ano</span><span class="sxs-lookup"><span data-stu-id="82287-146">Yes</span></span>                  | <span data-ttu-id="82287-147">Žádný k dispozici obsah, kódování není explicitně požadovaný</span><span class="sxs-lookup"><span data-stu-id="82287-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="82287-148">`Accept-Encoding` hodnoty hlavičky</span><span class="sxs-lookup"><span data-stu-id="82287-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="82287-149">Middleware podporována</span><span class="sxs-lookup"><span data-stu-id="82287-149">Middleware Supported</span></span> | <span data-ttu-id="82287-150">Popis</span><span class="sxs-lookup"><span data-stu-id="82287-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="82287-151">Ne</span><span class="sxs-lookup"><span data-stu-id="82287-151">No</span></span>                   | [<span data-ttu-id="82287-152">Formát komprimovaných dat Brotli</span><span class="sxs-lookup"><span data-stu-id="82287-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="82287-153">Ne</span><span class="sxs-lookup"><span data-stu-id="82287-153">No</span></span>                   | [<span data-ttu-id="82287-154">Formát komprimovaných dat DEFLATE</span><span class="sxs-lookup"><span data-stu-id="82287-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="82287-155">Ne</span><span class="sxs-lookup"><span data-stu-id="82287-155">No</span></span>                   | [<span data-ttu-id="82287-156">W3C XML efektivní výměny</span><span class="sxs-lookup"><span data-stu-id="82287-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="82287-157">Ano (výchozí)</span><span class="sxs-lookup"><span data-stu-id="82287-157">Yes (default)</span></span>        | [<span data-ttu-id="82287-158">Formát souborů GZIP</span><span class="sxs-lookup"><span data-stu-id="82287-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="82287-159">Ano</span><span class="sxs-lookup"><span data-stu-id="82287-159">Yes</span></span>                  | <span data-ttu-id="82287-160">"Bez kódování" identifikátor: odpověď nesmí být zakódován.</span><span class="sxs-lookup"><span data-stu-id="82287-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="82287-161">Ne</span><span class="sxs-lookup"><span data-stu-id="82287-161">No</span></span>                   | [<span data-ttu-id="82287-162">Formát přenosu sítě pro archivy Java</span><span class="sxs-lookup"><span data-stu-id="82287-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="82287-163">Ano</span><span class="sxs-lookup"><span data-stu-id="82287-163">Yes</span></span>                  | <span data-ttu-id="82287-164">Žádný k dispozici obsah, kódování není explicitně požadovaný</span><span class="sxs-lookup"><span data-stu-id="82287-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="82287-165">Další informace najdete v tématu [IANA oficiální kódování seznamu obsahu](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span><span class="sxs-lookup"><span data-stu-id="82287-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="82287-166">Middleware umožňuje přidat další komprese zprostředkovatelé pro vlastní `Accept-Encoding` hodnoty hlavičky.</span><span class="sxs-lookup"><span data-stu-id="82287-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="82287-167">Další informace najdete v tématu [Vlastní zprostředkovatelé](#custom-providers) níže.</span><span class="sxs-lookup"><span data-stu-id="82287-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="82287-168">Middleware je schopný reagovat na hodnotu kvality (qvalue, `q`) vážení při odeslání klientem nástroje k určení priority schémat komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="82287-169">Další informace najdete v tématu [RFC 7231: přijmout kódování](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span><span class="sxs-lookup"><span data-stu-id="82287-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="82287-170">Algoritmy komprese podléhají kompromis mezi komprese rychlost a efektivitu komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="82287-171">*Efektivnost definování* v tomto kontextu označuje velikost výstup po kompresi.</span><span class="sxs-lookup"><span data-stu-id="82287-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="82287-172">Nejmenší velikost se dosahuje nejčastěji *optimální* komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="82287-173">Záhlaví účastnící se požaduje, odesílání, ukládání do mezipaměti a přijímání komprimovaného obsahu jsou popsány v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="82287-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="82287-174">Záhlaví</span><span class="sxs-lookup"><span data-stu-id="82287-174">Header</span></span>             | <span data-ttu-id="82287-175">Role</span><span class="sxs-lookup"><span data-stu-id="82287-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="82287-176">Odeslaných z klienta na server k označení schémata přijatelné klientovi kódování obsahu.</span><span class="sxs-lookup"><span data-stu-id="82287-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="82287-177">Odeslaná ze serveru klientovi jako oznámení, kódování obsahu v datové části</span><span class="sxs-lookup"><span data-stu-id="82287-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="82287-178">Pokud dojde k kompresi, `Content-Length` záhlaví Odebereme, protože změny obsahu těla při kompresi odpovědi.</span><span class="sxs-lookup"><span data-stu-id="82287-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="82287-179">Pokud dojde k komprese, `Content-MD5` záhlaví Odebereme, protože došlo ke změně obsah textu a -the-hash již není platný.</span><span class="sxs-lookup"><span data-stu-id="82287-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="82287-180">Určuje typ MIME obsahu.</span><span class="sxs-lookup"><span data-stu-id="82287-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="82287-181">Každou odpověď by měla určit jeho `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="82287-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="82287-182">Middleware ověří tuto hodnotu k určení, pokud je nutné zkomprimovat odpovědi.</span><span class="sxs-lookup"><span data-stu-id="82287-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="82287-183">Middleware určuje sadu [výchozí typy MIME](#mime-types) , která můžete kódovat, ale můžete nahradit nebo přidat typy MIME.</span><span class="sxs-lookup"><span data-stu-id="82287-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="82287-184">Při odeslání server s hodnotou `Accept-Encoding` pro klienty a proxy servery, `Vary` hlavička označuje klienta nebo proxy server, který by měl mezipaměti (lišit) odpovědi na základě hodnoty z `Accept-Encoding` záhlaví požadavku.</span><span class="sxs-lookup"><span data-stu-id="82287-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="82287-185">Vrací obsah s výsledkem `Vary: Accept-Encoding` záhlaví je, že i komprimované a jsou v odpovědi na nekomprimované samostatně uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="82287-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="82287-186">Seznamte se s funkcemi Middleware pro kompresi odpovědí s [ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span><span class="sxs-lookup"><span data-stu-id="82287-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="82287-187">Ukázka znázorňuje:</span><span class="sxs-lookup"><span data-stu-id="82287-187">The sample illustrates:</span></span>

* <span data-ttu-id="82287-188">Kompresi odpovědí aplikace pomocí Gzip a poskytovatelé vlastní komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="82287-189">Jak přidat typ MIME do seznamu výchozích typů MIME pro kompresi.</span><span class="sxs-lookup"><span data-stu-id="82287-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="82287-190">Balíček</span><span class="sxs-lookup"><span data-stu-id="82287-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="82287-191">Aby byly middleware v projektu, přidejte odkaz na [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app), což zahrnuje [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="82287-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="82287-192">Aby byly middleware v projektu, přidejte odkaz na [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage), což zahrnuje [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="82287-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="82287-193">Aby byly middleware v projektu, přidejte odkaz na [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="82287-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="82287-194">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="82287-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="82287-195">Následující kód ukazuje, jak povolit Middleware pro kompresi odpovědí pro typy MIME výchozí a komprese poskytovatele ([Brotli](#brotli-compression-provider) a [Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="82287-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="82287-196">Následující kód ukazuje, jak povolit Middleware pro kompresi odpovědí pro typy MIME výchozí a [poskytovatele kompresi Gzip](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="82287-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

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
> <span data-ttu-id="82287-197">Pomocí některého nástroje, například [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), nebo [Postman](https://www.getpostman.com/) nastavit `Accept-Encoding` hlavička požadavku a studovat hlavičky odpovědi, velikost a text.</span><span class="sxs-lookup"><span data-stu-id="82287-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="82287-198">Odeslat žádost pro ukázkovou aplikaci bez `Accept-Encoding` záhlaví a podívejte se, že odpověď nekomprimované.</span><span class="sxs-lookup"><span data-stu-id="82287-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="82287-199">`Content-Encoding` a `Vary` záhlaví nejsou k dispozici v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="82287-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fiddler okno zobrazuje výsledek požadavku bez hlavičky Accept-Encoding.](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="82287-202">Odeslat žádost pro ukázkovou aplikaci s `Accept-Encoding: gzip` záhlaví a podívejte se, že je komprimován odpovědi.</span><span class="sxs-lookup"><span data-stu-id="82287-202">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="82287-203">`Content-Encoding` a `Vary` záhlaví jsou k dispozici v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="82287-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler okno zobrazuje výsledek žádosti s hlavičku Accept-Encoding a hodnotu gzip.](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="82287-207">Zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="82287-207">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="82287-208">Zprostředkovatel Brotli komprese</span><span class="sxs-lookup"><span data-stu-id="82287-208">Brotli Compression Provider</span></span>

<span data-ttu-id="82287-209">Použití `BrotliCompressionProvider` kompresi odpovědí s [Formát komprimovaných dat Brotli](https://tools.ietf.org/html/rfc7932).</span><span class="sxs-lookup"><span data-stu-id="82287-209">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="82287-210">Pokud žádný poskytovatel komprese jsou explicitně přidány do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="82287-210">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="82287-211">Zprostředkovatel komprese Brotli je přidán ve výchozím nastavení do pole komprese poskytovatelů spolu s [poskytovatele kompresi Gzip](#gzip-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="82287-211">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="82287-212">Výchozí nastavení komprese Brotli komprese při Brotli Formát komprimovaných dat je podporované klientem.</span><span class="sxs-lookup"><span data-stu-id="82287-212">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="82287-213">Pokud klient není podporován Brotli, komprese výchozí Gzip Pokud klient podporuje kompresi Gzip.</span><span class="sxs-lookup"><span data-stu-id="82287-213">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="82287-214">Zprostředkovatel Brotoli komprese musí být přidán, když jsou explicitně přidány všechny zprostředkovatele komprese:</span><span class="sxs-lookup"><span data-stu-id="82287-214">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

<span data-ttu-id="82287-215">Nastavit úroveň díky komprese `BrotliCompressionProviderOptions`.</span><span class="sxs-lookup"><span data-stu-id="82287-215">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="82287-216">Zprostředkovatel komprese Brotli výchozí hodnota je nejvyšší úroveň komprese ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), který nemusí vracet nejúčinnější komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-216">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="82287-217">V případě potřeby je nejúčinnější komprese, nakonfigurujte middleware pro kompresi optimální.</span><span class="sxs-lookup"><span data-stu-id="82287-217">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="82287-218">Úroveň komprese</span><span class="sxs-lookup"><span data-stu-id="82287-218">Compression Level</span></span> | <span data-ttu-id="82287-219">Popis</span><span class="sxs-lookup"><span data-stu-id="82287-219">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="82287-220">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="82287-220">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="82287-221">Komprese by se měla dokončit co nejrychleji i v případě, že výsledný výstup není komprimována optimálně.</span><span class="sxs-lookup"><span data-stu-id="82287-221">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="82287-222">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="82287-222">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="82287-223">Bez komprese je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="82287-223">No compression should be performed.</span></span> |
| [<span data-ttu-id="82287-224">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="82287-224">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="82287-225">Odpovědí by měl být optimálně komprimovaný, i v případě, že komprese trvá déle.</span><span class="sxs-lookup"><span data-stu-id="82287-225">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="82287-226">Zprostředkovatel kompresi GZIP</span><span class="sxs-lookup"><span data-stu-id="82287-226">Gzip Compression Provider</span></span>

<span data-ttu-id="82287-227">Použití <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> kompresi odpovědí s [formátu Gzip](https://tools.ietf.org/html/rfc1952).</span><span class="sxs-lookup"><span data-stu-id="82287-227">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="82287-228">Pokud žádný poskytovatel komprese jsou explicitně přidány do <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="82287-228">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="82287-229">Zprostředkovatel kompresi Gzip je přidat ve výchozím nastavení do pole, poskytovatelů komprese spolu s [Brotli komprese poskytovatele](#brotli-compression-provider).</span><span class="sxs-lookup"><span data-stu-id="82287-229">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="82287-230">Výchozí nastavení komprese Brotli komprese při Brotli Formát komprimovaných dat je podporované klientem.</span><span class="sxs-lookup"><span data-stu-id="82287-230">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="82287-231">Pokud klient není podporován Brotli, komprese výchozí Gzip Pokud klient podporuje kompresi Gzip.</span><span class="sxs-lookup"><span data-stu-id="82287-231">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="82287-232">Zprostředkovatel kompresi Gzip je ve výchozím nastavení do pole komprese poskytovatelů.</span><span class="sxs-lookup"><span data-stu-id="82287-232">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="82287-233">Výchozí hodnota kompresi Gzip, pokud klient podporuje kompresi Gzip.</span><span class="sxs-lookup"><span data-stu-id="82287-233">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="82287-234">Zprostředkovatel kompresi Gzip musí být přidán, když jsou explicitně přidány všechny zprostředkovatele komprese:</span><span class="sxs-lookup"><span data-stu-id="82287-234">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="82287-235">Nastavit úroveň díky komprese <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span><span class="sxs-lookup"><span data-stu-id="82287-235">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="82287-236">Zprostředkovatel kompresi Gzip výchozí hodnota je nejvyšší úroveň komprese ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), který nemusí vracet nejúčinnější komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-236">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="82287-237">V případě potřeby je nejúčinnější komprese, nakonfigurujte middleware pro kompresi optimální.</span><span class="sxs-lookup"><span data-stu-id="82287-237">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="82287-238">Úroveň komprese</span><span class="sxs-lookup"><span data-stu-id="82287-238">Compression Level</span></span> | <span data-ttu-id="82287-239">Popis</span><span class="sxs-lookup"><span data-stu-id="82287-239">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="82287-240">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="82287-240">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="82287-241">Komprese by se měla dokončit co nejrychleji i v případě, že výsledný výstup není komprimována optimálně.</span><span class="sxs-lookup"><span data-stu-id="82287-241">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="82287-242">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="82287-242">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="82287-243">Bez komprese je třeba provést.</span><span class="sxs-lookup"><span data-stu-id="82287-243">No compression should be performed.</span></span> |
| [<span data-ttu-id="82287-244">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="82287-244">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="82287-245">Odpovědí by měl být optimálně komprimovaný, i v případě, že komprese trvá déle.</span><span class="sxs-lookup"><span data-stu-id="82287-245">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="82287-246">Vlastní zprostředkovatelé</span><span class="sxs-lookup"><span data-stu-id="82287-246">Custom providers</span></span>

<span data-ttu-id="82287-247">Vytvořit vlastní komprese implementace s <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span><span class="sxs-lookup"><span data-stu-id="82287-247">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="82287-248"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> Reprezentuje obsah, kódování, že tento `ICompressionProvider` vytvoří.</span><span class="sxs-lookup"><span data-stu-id="82287-248">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="82287-249">Middleware použije tyto informace k výběru poskytovatele podle zadaného v seznamu `Accept-Encoding` záhlaví požadavku.</span><span class="sxs-lookup"><span data-stu-id="82287-249">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="82287-250">Pomocí ukázkové aplikace, klient odešle požadavek s `Accept-Encoding: mycustomcompression` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="82287-250">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="82287-251">Middleware použije implementace vlastní komprese a vrátí odpověď se `Content-Encoding: mycustomcompression` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="82287-251">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="82287-252">Klient musí být schopen dekomprimovat vlastní kódování v pořadí pro implementaci vlastního komprese pracovat.</span><span class="sxs-lookup"><span data-stu-id="82287-252">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="82287-253">Odeslat žádost pro ukázkovou aplikaci s `Accept-Encoding: mycustomcompression` záhlaví a sledujte hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="82287-253">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="82287-254">`Vary` a `Content-Encoding` záhlaví jsou k dispozici v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="82287-254">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="82287-255">Text odpovědi (není vidět) není komprimována v rámci ukázky.</span><span class="sxs-lookup"><span data-stu-id="82287-255">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="82287-256">Není k dispozici implementace komprese ve `CustomCompressionProvider` třídy vzorku.</span><span class="sxs-lookup"><span data-stu-id="82287-256">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="82287-257">Ale tato ukázka vysvětluje, kdy, měli byste implementovat algoritmus komprese.</span><span class="sxs-lookup"><span data-stu-id="82287-257">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler okno zobrazuje výsledek žádosti s hlavičku Accept-Encoding a hodnotu mycustomcompression.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="82287-260">typy MIME</span><span class="sxs-lookup"><span data-stu-id="82287-260">MIME types</span></span>

<span data-ttu-id="82287-261">Middleware Určuje výchozí sadu typů MIME pro komprese:</span><span class="sxs-lookup"><span data-stu-id="82287-261">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="82287-262">Nahradit nebo přidat typy MIME s možnostmi Middleware pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="82287-262">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="82287-263">Všimněte si danému zástupnému znaku MIME typy, jako například `text/*` nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="82287-263">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="82287-264">Ukázková aplikace přidá typ MIME pro `image/svg+xml` a komprimuje a ASP.NET Core slouží banner image (*banner.svg*).</span><span class="sxs-lookup"><span data-stu-id="82287-264">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="82287-265">Komprese se zabezpečený protokol</span><span class="sxs-lookup"><span data-stu-id="82287-265">Compression with secure protocol</span></span>

<span data-ttu-id="82287-266">Komprimované odpovědi prostřednictvím zabezpečeného připojení se dá řídit pomocí `EnableForHttps` možnost, která je ve výchozím nastavení zakázané.</span><span class="sxs-lookup"><span data-stu-id="82287-266">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="82287-267">Pomocí komprese dynamicky generované stránky může vést k problémům zabezpečení, jako [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) a [porušení](https://wikipedia.org/wiki/BREACH_(security_exploit)) útoky.</span><span class="sxs-lookup"><span data-stu-id="82287-267">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="82287-268">Přidání měnit hlavičky</span><span class="sxs-lookup"><span data-stu-id="82287-268">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="82287-269">Při kompresi odpovědí na základě `Accept-Encoding` záhlaví, jsou potenciálně více komprimované verze odpovědi a nekomprimované verze.</span><span class="sxs-lookup"><span data-stu-id="82287-269">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="82287-270">Aby bylo možné, že více verzí existují a by měla být uložena, dát pokyn mezipaměti klienta a serveru proxy `Vary` záhlaví se přidá s `Accept-Encoding` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="82287-270">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="82287-271">V technologii ASP.NET Core 2.0 nebo novější, přidá middleware `Vary` záhlaví automaticky, pokud je odpověď je komprimován.</span><span class="sxs-lookup"><span data-stu-id="82287-271">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="82287-272">Při kompresi odpovědí na základě `Accept-Encoding` záhlaví, jsou potenciálně více komprimované verze odpovědi a nekomprimované verze.</span><span class="sxs-lookup"><span data-stu-id="82287-272">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="82287-273">Aby bylo možné, že více verzí existují a by měla být uložena, dát pokyn mezipaměti klienta a serveru proxy `Vary` záhlaví se přidá s `Accept-Encoding` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="82287-273">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="82287-274">V ASP.NET Core 1.x, přidání `Vary` hlavičky odpovědi se provádí ručně:</span><span class="sxs-lookup"><span data-stu-id="82287-274">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="82287-275">Middleware problém při za reverzní proxy server Nginx</span><span class="sxs-lookup"><span data-stu-id="82287-275">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="82287-276">Pokud je požadavek směrovány přes proxy server pomocí Nginx, `Accept-Encoding` odebrat záhlaví.</span><span class="sxs-lookup"><span data-stu-id="82287-276">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="82287-277">To zabrání middleware komprese odpovědi.</span><span class="sxs-lookup"><span data-stu-id="82287-277">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="82287-278">Další informace najdete v tématu [NGINX: komprese a dekomprese](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span><span class="sxs-lookup"><span data-stu-id="82287-278">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="82287-279">Tento problém je sledován pomocí funkce [zjistit průchozí komprese Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span><span class="sxs-lookup"><span data-stu-id="82287-279">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="82287-280">Práce s dynamické komprese služby IIS</span><span class="sxs-lookup"><span data-stu-id="82287-280">Working with IIS dynamic compression</span></span>

<span data-ttu-id="82287-281">Pokud máte aktivní IIS dynamické komprese modul nakonfigurována na úrovni serveru, který chcete zakázat aplikaci zakázat modul s parametrem doplněk *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="82287-281">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="82287-282">Další informace najdete v tématu [moduly IIS zakázání](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span><span class="sxs-lookup"><span data-stu-id="82287-282">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="82287-283">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="82287-283">Troubleshooting</span></span>

<span data-ttu-id="82287-284">Pomocí některého nástroje, například [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), nebo [Postman](https://www.getpostman.com/), což umožní nastavit `Accept-Encoding` hlavička požadavku a studovat hlavičky odpovědi, velikost a text.</span><span class="sxs-lookup"><span data-stu-id="82287-284">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="82287-285">Ve výchozím nastavení komprimuje Middleware pro kompresi odpovědí odpovědi, které splňovat tyto podmínky:</span><span class="sxs-lookup"><span data-stu-id="82287-285">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="82287-286">`Accept-Encoding` Záhlaví je k dispozici s hodnotou `br`, `gzip`, `*`, nebo vlastní kódování, který odpovídá vlastní komprese poskytovatele, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="82287-286">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="82287-287">Hodnota nesmí být `identity` nebo mít hodnotu kvality (qvalue, `q`) nastavení 0 (nula).</span><span class="sxs-lookup"><span data-stu-id="82287-287">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="82287-288">Typ MIME (`Content-Type`) musí být nastavena a musí odpovídat typu MIME na nakonfigurované <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="82287-288">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="82287-289">Nesmí obsahovat požadavek `Content-Range` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="82287-289">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="82287-290">Žádost musí používat zabezpečený protokol (http), pokud zabezpečený protokol (https) je nakonfigurován v možnostech Middleware pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="82287-290">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="82287-291">*Všimněte si, nebezpečí [výše popsané](#compression-with-secure-protocol) při povolování zabezpečené komprese obsahu.*</span><span class="sxs-lookup"><span data-stu-id="82287-291">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="82287-292">`Accept-Encoding` Záhlaví je k dispozici s hodnotou `gzip`, `*`, nebo vlastní kódování, který odpovídá vlastní komprese poskytovatele, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="82287-292">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="82287-293">Hodnota nesmí být `identity` nebo mít hodnotu kvality (qvalue, `q`) nastavení 0 (nula).</span><span class="sxs-lookup"><span data-stu-id="82287-293">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="82287-294">Typ MIME (`Content-Type`) musí být nastavena a musí odpovídat typu MIME na nakonfigurované <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span><span class="sxs-lookup"><span data-stu-id="82287-294">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="82287-295">Nesmí obsahovat požadavek `Content-Range` záhlaví.</span><span class="sxs-lookup"><span data-stu-id="82287-295">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="82287-296">Žádost musí používat zabezpečený protokol (http), pokud zabezpečený protokol (https) je nakonfigurován v možnostech Middleware pro kompresi odpovědí.</span><span class="sxs-lookup"><span data-stu-id="82287-296">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="82287-297">*Všimněte si, nebezpečí [výše popsané](#compression-with-secure-protocol) při povolování zabezpečené komprese obsahu.*</span><span class="sxs-lookup"><span data-stu-id="82287-297">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="82287-298">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="82287-298">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="82287-299">Mozilla Developer Network: Přijměte – kódování</span><span class="sxs-lookup"><span data-stu-id="82287-299">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="82287-300">RFC 7231 části 3.1.2.1: Obsahu Codings</span><span class="sxs-lookup"><span data-stu-id="82287-300">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="82287-301">RFC 7230 oddílu 4.2.3: Gzip kódování</span><span class="sxs-lookup"><span data-stu-id="82287-301">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="82287-302">Verze specifikace formátu souboru GZIP 4.3</span><span class="sxs-lookup"><span data-stu-id="82287-302">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
