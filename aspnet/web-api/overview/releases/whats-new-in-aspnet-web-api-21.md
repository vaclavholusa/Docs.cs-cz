---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Co je nového v rozhraní ASP.NET Web API 2.1 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="00b6a-102">Co je nového v rozhraní ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="00b6a-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="00b6a-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="00b6a-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="00b6a-104">Toto téma popisuje, co je nového pro ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="00b6a-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="00b6a-105">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="00b6a-105">Download</span></span>](#download)
- [<span data-ttu-id="00b6a-106">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="00b6a-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="00b6a-107">Nové funkce v rozhraní ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="00b6a-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="00b6a-108">Zpracování chyb globální</span><span class="sxs-lookup"><span data-stu-id="00b6a-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="00b6a-109">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="00b6a-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="00b6a-110">Vylepšení stránky nápovědy</span><span class="sxs-lookup"><span data-stu-id="00b6a-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="00b6a-111">Podpora IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="00b6a-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="00b6a-112">Formátovací modul typu média BSON</span><span class="sxs-lookup"><span data-stu-id="00b6a-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="00b6a-113">Lepší podporu pro asynchronní filtry</span><span class="sxs-lookup"><span data-stu-id="00b6a-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="00b6a-114">Analýza pro klienta formátování knihovny dotazu</span><span class="sxs-lookup"><span data-stu-id="00b6a-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="00b6a-115">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="00b6a-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="00b6a-116">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="00b6a-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="00b6a-117">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="00b6a-117">Download</span></span>

<span data-ttu-id="00b6a-118">Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="00b6a-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="00b6a-119">Všechny balíčky modulu runtime použijte [sémantické verze](http://semver.org/) specifikace.</span><span class="sxs-lookup"><span data-stu-id="00b6a-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="00b6a-120">Nejnovější balíček ASP.NET Web API 2.1 RTM má následující verze: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="00b6a-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="00b6a-121">Můžete instalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="00b6a-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="00b6a-122">Verze také zahrnuje odpovídající lokalizované balíčky na NuGet.</span><span class="sxs-lookup"><span data-stu-id="00b6a-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="00b6a-123">Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="00b6a-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="00b6a-124">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="00b6a-124">Documentation</span></span>

<span data-ttu-id="00b6a-125">Kurzy a další informace o ASP.NET Web API 2.1 RTM jsou dostupné na webu technologie ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="00b6a-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="00b6a-126">Nové funkce v rozhraní ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="00b6a-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="00b6a-127">Zpracování chyb globální</span><span class="sxs-lookup"><span data-stu-id="00b6a-127">Global Error Handling</span></span>

<span data-ttu-id="00b6a-128">Přes jednu centrální mechanismus lze nyní protokolovat všechny neošetřené výjimky a lze přizpůsobit chování neošetřených výjimek.</span><span class="sxs-lookup"><span data-stu-id="00b6a-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="00b6a-129">Rozhraní framework podporuje více protokolovačů výjimek, které najdete v části neošetřené výjimky a informace o kontextu, ve kterém je došlo, jako je například požadavek zpracovávaný v době.</span><span class="sxs-lookup"><span data-stu-id="00b6a-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="00b6a-130">Například následující kód používá System.Diagnostics.TraceSource k protokolování všech neošetřených výjimek:</span><span class="sxs-lookup"><span data-stu-id="00b6a-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="00b6a-131">Můžete také nahradit výchozí obslužnou rutinu výjimky, takže můžete plně přizpůsobit zpráva odpovědi HTTP, která je odeslána, když k neošetřené výjimce dochází.</span><span class="sxs-lookup"><span data-stu-id="00b6a-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="00b6a-132">Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) který protokoluje všechny nezpracované výjimky pomocí Oblíbené ELMAH framework.</span><span class="sxs-lookup"><span data-stu-id="00b6a-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="00b6a-133">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="00b6a-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="00b6a-134">Směrováním atributů podporuje omezení, povolení správy verzí a výběru na základě záhlaví trasy.</span><span class="sxs-lookup"><span data-stu-id="00b6a-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="00b6a-135">Mnoho aspektů trasy atributů jsou také nyní přizpůsobit pomocí **IDirectRouteFactory** rozhraní a **RouteFactoryAttribute** třídy.</span><span class="sxs-lookup"><span data-stu-id="00b6a-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="00b6a-136">Předpona trasy je nyní extensible prostřednictvím **IRoutePrefix** rozhraní a **RoutePrefixAttribute** třídy.</span><span class="sxs-lookup"><span data-stu-id="00b6a-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="00b6a-137">Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) používající omezení dynamicky řadiče filtrovat podle záhlaví HTTP 'api-version'.</span><span class="sxs-lookup"><span data-stu-id="00b6a-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="00b6a-138">Vylepšení stránky nápovědy</span><span class="sxs-lookup"><span data-stu-id="00b6a-138">Help Page Improvements</span></span>

<span data-ttu-id="00b6a-139">Webové rozhraní API 2.1 obsahuje následující vylepšení k [stránky nápovědy rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="00b6a-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="00b6a-140">Dokumentace jednotlivé vlastnosti parametrů nebo návratové typy akcí.</span><span class="sxs-lookup"><span data-stu-id="00b6a-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="00b6a-141">Dokumentace anotací dat modelu.</span><span class="sxs-lookup"><span data-stu-id="00b6a-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="00b6a-142">Návrh uživatelského rozhraní stránky nápovědy také aktualizován, aby dokázala pojmout tyto změny.</span><span class="sxs-lookup"><span data-stu-id="00b6a-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="00b6a-143">Podpora IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="00b6a-143">IgnoreRoute Support</span></span>

<span data-ttu-id="00b6a-144">Webové rozhraní API 2.1 podporuje ignoruje vzorů adresy URL v směrování rozhraní Web API pomocí sady **IgnoreRoute** rozšiřující metody na **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="00b6a-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="00b6a-145">Tyto metody způsobit webového rozhraní API ignorovat všechny adresy URL, které odpovídají zadané šablony a povolit na hostiteli a poté použít další zpracování, je-li to vhodné.</span><span class="sxs-lookup"><span data-stu-id="00b6a-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="00b6a-146">Následující příklad ignoruje identifikátory URI, který bude začínat &quot;obsah&quot; segmentu:</span><span class="sxs-lookup"><span data-stu-id="00b6a-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="00b6a-147">Formátovací modul typu média BSON</span><span class="sxs-lookup"><span data-stu-id="00b6a-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="00b6a-148">Webové rozhraní API nyní podporuje [BSON](http://bsonspec.org/) přenosový formát, v klientovi i na serveru.</span><span class="sxs-lookup"><span data-stu-id="00b6a-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="00b6a-149">Chcete-li povolit BSON na straně serveru, přidejte **BsonMediaTypeFormatter** do kolekce formátovacích modulů:</span><span class="sxs-lookup"><span data-stu-id="00b6a-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="00b6a-150">Tady je způsob, jakým může klient .NET využívají formátu BSON:</span><span class="sxs-lookup"><span data-stu-id="00b6a-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="00b6a-151">Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) který ukazuje straně klient i server.</span><span class="sxs-lookup"><span data-stu-id="00b6a-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="00b6a-152">Další informace najdete v tématu [BSON podpory v 2.1 webové rozhraní API](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="00b6a-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="00b6a-153">Lepší podporu pro asynchronní filtry</span><span class="sxs-lookup"><span data-stu-id="00b6a-153">Better Support for Async Filters</span></span>

<span data-ttu-id="00b6a-154">Webové rozhraní API teď podporuje snadný způsob, jak vytvářet filtry, které se spouští asynchronně.</span><span class="sxs-lookup"><span data-stu-id="00b6a-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="00b6a-155">Tato funkce je užitečná je filtr potřebuje k provedení asynchronní akce, jako je přístup a databáze.</span><span class="sxs-lookup"><span data-stu-id="00b6a-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="00b6a-156">Dříve Pokud chcete vytvořit filtr asynchronní, jste museli implementovat rozhraní filtru, sami, protože základní třídy filtru dostupná jenom v případě synchronních metod.</span><span class="sxs-lookup"><span data-stu-id="00b6a-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="00b6a-157">Teď můžete přepsat virtuální `On*Async` metody filtru základní třídy.</span><span class="sxs-lookup"><span data-stu-id="00b6a-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="00b6a-158">Příklad:</span><span class="sxs-lookup"><span data-stu-id="00b6a-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="00b6a-159">**AuthorizationFilterAttribute**, **ActionFilterAttribute**, a **ExceptionFilterAttribute** třídy všechny podporují asynchronní v 2.1 webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="00b6a-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="00b6a-160">Analýza pro klienta formátování knihovny dotazu</span><span class="sxs-lookup"><span data-stu-id="00b6a-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="00b6a-161">Dříve **System.Net.Http.Formatting** podporuje analýzy a aktualizaci dotazy identifikátoru URI pro kódu na straně serveru, ale ekvivalentní přenosné knihovny chyběl tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="00b6a-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="00b6a-162">Ve webové rozhraní API 2.1 klientskou aplikaci teď můžete snadno analyzovat a aktualizovat řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="00b6a-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="00b6a-163">Následující příklady ukazují, jak analyzovat, úpravě a vygenerovat URI dotazy.</span><span class="sxs-lookup"><span data-stu-id="00b6a-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="00b6a-164">(V příkladech konzolovou aplikaci pro jednoduchost.)</span><span class="sxs-lookup"><span data-stu-id="00b6a-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="00b6a-165">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="00b6a-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="00b6a-166">Tato část popisuje známé problémy a nejnovějších změn v RTM k 2.1 aplikace ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="00b6a-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="00b6a-167">Atribut směrování</span><span class="sxs-lookup"><span data-stu-id="00b6a-167">Attribute Routing</span></span>

<span data-ttu-id="00b6a-168">Mnohoznačnosti v atributu směrování odpovídá teď chybu místo výběru na první shodu.</span><span class="sxs-lookup"><span data-stu-id="00b6a-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="00b6a-169">Atribut trasy mají zakázáno používat *{controller}* parametr a pomocí *{action}* parametr v směrování umístit na akce.</span><span class="sxs-lookup"><span data-stu-id="00b6a-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="00b6a-170">Tyto parametry velmi pravděpodobně by způsobilo nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="00b6a-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="00b6a-171">Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.1 výsledky balíčky v 5.0 balíčky pro šablony, které již nejsou v projektu</span><span class="sxs-lookup"><span data-stu-id="00b6a-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="00b6a-172">Aktualizace balíčků NuGet pro ASP.NET Web API 2.1 RTM neaktualizuje nástrojů Visual Studio, jako je ASP.NET generování uživatelského rozhraní nebo šablona projektu webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="00b6a-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="00b6a-173">Používají předchozí verzi modulu runtime ASP.NET balíčky (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="00b6a-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="00b6a-174">Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (5.0.0.0) požadované balíčky, pokud již nejsou k dispozici v projektech.</span><span class="sxs-lookup"><span data-stu-id="00b6a-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="00b6a-175">ASP.NET generování uživatelského rozhraní v Visual Studio 2013 RTM nebo Update 1, ale nepřepíše nejnovější balíčků ve vašich projektů.</span><span class="sxs-lookup"><span data-stu-id="00b6a-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="00b6a-176">Pokud používáte generování uživatelského rozhraní ASP.NET po aktualizaci balíčků pro webové rozhraní API 2.1 nebo ASP.NET MVC 5.1, ujistěte se, že verze webového rozhraní API a MVC jsou konzistentní.</span><span class="sxs-lookup"><span data-stu-id="00b6a-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="00b6a-177">Přejmenuje typu</span><span class="sxs-lookup"><span data-stu-id="00b6a-177">Type Renames</span></span>

<span data-ttu-id="00b6a-178">Některé typy používané pro atribut směrování rozšiřitelnost byly přejmenován ze RC na 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="00b6a-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="00b6a-179">Původní název typu (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="00b6a-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="00b6a-180">Název nového typu (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="00b6a-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="00b6a-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="00b6a-181">IDirectRouteProvider</span></span> | <span data-ttu-id="00b6a-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="00b6a-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="00b6a-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="00b6a-183">RouteProviderAttribute</span></span> | <span data-ttu-id="00b6a-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="00b6a-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="00b6a-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="00b6a-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="00b6a-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="00b6a-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="00b6a-187">Filtry výjimek rozbalení není agregační výjimky vzniklé v asynchronní akce</span><span class="sxs-lookup"><span data-stu-id="00b6a-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="00b6a-188">Dříve Pokud v asynchronní akce došlo **AggregateException**, filtr výjimek by rozbalení výjimka, a **OnException** by získat základní výjimky.</span><span class="sxs-lookup"><span data-stu-id="00b6a-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="00b6a-189">V 2.1, filtr výjimek není rozbalení, a **OnException** získá původní **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="00b6a-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="00b6a-190">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="00b6a-190">Bug Fixes</span></span>

<span data-ttu-id="00b6a-191">Tato verze rovněž obsahuje několik oprav chyb.</span><span class="sxs-lookup"><span data-stu-id="00b6a-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="00b6a-192">Úplný seznam najdete:</span><span class="sxs-lookup"><span data-stu-id="00b6a-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="00b6a-193">5.1.0 balíček</span><span class="sxs-lookup"><span data-stu-id="00b6a-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="00b6a-194">5.1.1 balíčku</span><span class="sxs-lookup"><span data-stu-id="00b6a-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="00b6a-195">5.1.2 balíček obsahuje aktualizace IntelliSense, ale žádné opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="00b6a-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
