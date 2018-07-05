---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Co je nového v rozhraní ASP.NET Web API 2.1 | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 8e0501570e6dc6a9a6f69a642f9ab031c5497b5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385697"
---
<a name="whats-new-in-aspnet-web-api-21"></a><span data-ttu-id="8c042-102">Co je nového v rozhraní ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="8c042-102">What's New in ASP.NET Web API 2.1</span></span>
====================
<span data-ttu-id="8c042-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8c042-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="8c042-104">Toto téma popisuje, co je nového v ASP.NET Web API 2.1.</span><span class="sxs-lookup"><span data-stu-id="8c042-104">This topic describes what's new for ASP.NET Web API 2.1.</span></span>

- [<span data-ttu-id="8c042-105">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="8c042-105">Download</span></span>](#download)
- [<span data-ttu-id="8c042-106">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="8c042-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="8c042-107">Nové funkce v rozhraní ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="8c042-107">New Features in ASP.NET Web API 2.1</span></span>](#new-features)

    - [<span data-ttu-id="8c042-108">Zpracování chyb globální</span><span class="sxs-lookup"><span data-stu-id="8c042-108">Global Error Handling</span></span>](#global-error)
    - [<span data-ttu-id="8c042-109">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="8c042-109">Attribute Routing Improvements</span></span>](#attribute-routing)
    - [<span data-ttu-id="8c042-110">Vylepšení stránky nápovědy</span><span class="sxs-lookup"><span data-stu-id="8c042-110">Help Page Improvements</span></span>](#help-page)
    - [<span data-ttu-id="8c042-111">Podpora IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="8c042-111">IgnoreRoute Support</span></span>](#ignoreroute)
    - [<span data-ttu-id="8c042-112">Formátovací modul typu média BSON</span><span class="sxs-lookup"><span data-stu-id="8c042-112">BSON Media-Type Formatter</span></span>](#bson)
    - [<span data-ttu-id="8c042-113">Lepší podporu pro asynchronní filtry</span><span class="sxs-lookup"><span data-stu-id="8c042-113">Better Support for Async Filters</span></span>](#async-filters)
    - [<span data-ttu-id="8c042-114">Analýza kódu pro formátování knihovna klienta dotazu</span><span class="sxs-lookup"><span data-stu-id="8c042-114">Query Parsing for the Client Formatting Library</span></span>](#query-parsing)
- [<span data-ttu-id="8c042-115">Známé problémy a změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="8c042-115">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="8c042-116">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="8c042-116">Bug Fixes</span></span>](#bug-fixes)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="8c042-117">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="8c042-117">Download</span></span>

<span data-ttu-id="8c042-118">Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="8c042-118">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="8c042-119">Postupujte podle všech balíčků modulu runtime [Semantic Versioning](http://semver.org/) specifikace.</span><span class="sxs-lookup"><span data-stu-id="8c042-119">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="8c042-120">Nejnovější balíček ASP.NET Web API 2.1 RTM má následující verzi: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="8c042-120">The latest ASP.NET Web API 2.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="8c042-121">Můžete nainstalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="8c042-121">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="8c042-122">Tato verze rovněž obsahuje odpovídající lokalizované balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="8c042-122">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="8c042-123">Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="8c042-123">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="8c042-124">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="8c042-124">Documentation</span></span>

<span data-ttu-id="8c042-125">Kurzy a další informace o ASP.NET Web API 2.1 RTM jsou k dispozici z webu technologie ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="8c042-125">Tutorials and other information about ASP.NET Web API 2.1 RTM are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a><span data-ttu-id="8c042-126">Nové funkce v rozhraní ASP.NET Web API 2.1</span><span class="sxs-lookup"><span data-stu-id="8c042-126">New Features in ASP.NET Web API 2.1</span></span>

<a id="global-error"></a>
### <a name="global-error-handling"></a><span data-ttu-id="8c042-127">Zpracování chyb globální</span><span class="sxs-lookup"><span data-stu-id="8c042-127">Global Error Handling</span></span>

<span data-ttu-id="8c042-128">Prostřednictvím jedné centrální mechanismus můžete nyní protokolovat všechny neošetřené výjimky a je možné přizpůsobit chování neošetřené výjimky.</span><span class="sxs-lookup"><span data-stu-id="8c042-128">All unhandled exceptions can now be logged through one central mechanism, and the behavior for unhandled exceptions can be customized.</span></span>

<span data-ttu-id="8c042-129">Rozhraní framework podporuje více protokolovačů výjimek, které najdete v článku neošetřené výjimky a informace o kontextu, ve kterém k němu došlo, jako je například požadavek zpracovávaný při.</span><span class="sxs-lookup"><span data-stu-id="8c042-129">The framework supports multiple exception loggers, which all see the unhandled exception and information about the context in which it occurred, such as the request being processed at the time.</span></span>

<span data-ttu-id="8c042-130">Například následující kód používá System.Diagnostics.TraceSource do protokolu všechny neošetřené výjimky:</span><span class="sxs-lookup"><span data-stu-id="8c042-130">For example, the following code uses System.Diagnostics.TraceSource to log all unhandled exceptions:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

<span data-ttu-id="8c042-131">Můžete také nahradit výchozí obslužnou rutinu výjimky, takže můžete plně přizpůsobit zprávy s odpovědí HTTP, který se odešle, když k neošetřené výjimce dojde.</span><span class="sxs-lookup"><span data-stu-id="8c042-131">You can also replace the default exception handler, so that you can fully customize the HTTP response message that is sent when an unhandled exception occurs.</span></span>

<span data-ttu-id="8c042-132">Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , která protokoluje všechny neošetřené výjimky pomocí oblíbených rozhraní ELMAH.</span><span class="sxs-lookup"><span data-stu-id="8c042-132">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) that logs all unhandled exceptions via the popular ELMAH framework.</span></span>

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="8c042-133">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="8c042-133">Attribute Routing Improvements</span></span>

<span data-ttu-id="8c042-134">Směrování atributů podporuje omezení, povolení správy verzí a výběr založeným na hlavičkách trasy.</span><span class="sxs-lookup"><span data-stu-id="8c042-134">Attribute routing now supports constraints, enabling versioning and header-based route selection.</span></span> <span data-ttu-id="8c042-135">Mnoho aspektů trasy atributů jsou také nyní přizpůsobit prostřednictvím **IDirectRouteFactory** rozhraní a **RouteFactoryAttribute** třídy.</span><span class="sxs-lookup"><span data-stu-id="8c042-135">Also, many aspects of attribute routes are now customizable via the **IDirectRouteFactory** interface and **RouteFactoryAttribute** class.</span></span> <span data-ttu-id="8c042-136">Předpona trasy je rozšiřitelný prostřednictvím **IRoutePrefix** rozhraní a **RoutePrefixAttribute** třídy.</span><span class="sxs-lookup"><span data-stu-id="8c042-136">The route prefix is now extensible via the **IRoutePrefix** interface and **RoutePrefixAttribute** class.</span></span>

<span data-ttu-id="8c042-137">Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) omezení, která používá dynamicky řadiče filtrovat podle verze api-version záhlaví HTTP.</span><span class="sxs-lookup"><span data-stu-id="8c042-137">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) that uses constraints to dynamically filter controllers by an 'api-version' HTTP header.</span></span>

<a id="help-page"></a>
### <a name="help-page-improvements"></a><span data-ttu-id="8c042-138">Vylepšení stránky nápovědy</span><span class="sxs-lookup"><span data-stu-id="8c042-138">Help Page Improvements</span></span>

<span data-ttu-id="8c042-139">Webové rozhraní API 2.1 obsahuje následující vylepšení [stránky nápovědy k rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span><span class="sxs-lookup"><span data-stu-id="8c042-139">Web API 2.1 includes the following enhancements to [API Help Pages](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):</span></span>

- <span data-ttu-id="8c042-140">Dokumentace ke službě jednotlivé vlastnosti parametry nebo návratové typy akcí.</span><span class="sxs-lookup"><span data-stu-id="8c042-140">Documentation of individual properties of parameters or return types of actions.</span></span>
- <span data-ttu-id="8c042-141">Dokumentace ke službě anotacemi dat modelu.</span><span class="sxs-lookup"><span data-stu-id="8c042-141">Documentation of data model annotations.</span></span>

<span data-ttu-id="8c042-142">Návrh uživatelského rozhraní stránky nápovědy byla také aktualizována, aby tyto změny.</span><span class="sxs-lookup"><span data-stu-id="8c042-142">The UI design of the help pages was also updated, to accomodate these changes.</span></span>

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a><span data-ttu-id="8c042-143">Podpora IgnoreRoute</span><span class="sxs-lookup"><span data-stu-id="8c042-143">IgnoreRoute Support</span></span>

<span data-ttu-id="8c042-144">Webové rozhraní API 2.1 podporuje ignoruje vzory adresy URL webového rozhraní API směrování přes sadu **IgnoreRoute** rozšiřující metody na **HttpRouteCollection**.</span><span class="sxs-lookup"><span data-stu-id="8c042-144">Web API 2.1 supports ignoring URL patterns in Web API routing, through a set of **IgnoreRoute** extension methods on **HttpRouteCollection**.</span></span> <span data-ttu-id="8c042-145">Tyto metody způsobit, že webové rozhraní API ignorovat všechny adresy URL, které odpovídají zadané šablony a povolit hostitele použít další zpracování, pokud je to vhodné.</span><span class="sxs-lookup"><span data-stu-id="8c042-145">These methods cause Web API to ignore any URLs that match a specified template, and allow the host to apply additional processing if appropriate.</span></span>

<span data-ttu-id="8c042-146">Následující příklad ignoruje identifikátory URI, které začínají &quot;obsah&quot; segmentu:</span><span class="sxs-lookup"><span data-stu-id="8c042-146">The following example ignores URIs that start with a &quot;content&quot; segment:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a><span data-ttu-id="8c042-147">Formátovací modul typu média BSON</span><span class="sxs-lookup"><span data-stu-id="8c042-147">BSON Media-Type Formatter</span></span>

<span data-ttu-id="8c042-148">Webové rozhraní API teď podporuje [BSON](http://bsonspec.org/) přenosový formát, na straně klienta i na serveru.</span><span class="sxs-lookup"><span data-stu-id="8c042-148">Web API now supports the [BSON](http://bsonspec.org/) wire format, both on the client and on the server.</span></span>

<span data-ttu-id="8c042-149">Pokud chcete povolit BSON na straně serveru, přidejte **BsonMediaTypeFormatter** do kolekce formátovacích modulů:</span><span class="sxs-lookup"><span data-stu-id="8c042-149">To enable BSON on the server side, add the **BsonMediaTypeFormatter** to the formatters collection:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

<span data-ttu-id="8c042-150">Zde je, jak klient .NET mohou využívat formátu BSON:</span><span class="sxs-lookup"><span data-stu-id="8c042-150">Here is how a .NET client can consume BSON format:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

<span data-ttu-id="8c042-151">Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) , který ukazuje na straně klienta i serveru.</span><span class="sxs-lookup"><span data-stu-id="8c042-151">We have provided a [sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) that shows both the client and server side.</span></span>

<span data-ttu-id="8c042-152">Další informace najdete v tématu [podpora formátu BSON ve webové rozhraní API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span><span class="sxs-lookup"><span data-stu-id="8c042-152">For more information, see [BSON Support in Web API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)</span></span>

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a><span data-ttu-id="8c042-153">Lepší podporu pro asynchronní filtry</span><span class="sxs-lookup"><span data-stu-id="8c042-153">Better Support for Async Filters</span></span>

<span data-ttu-id="8c042-154">Webové rozhraní API teď podporuje snadný způsob, jak vytvářet filtry, které jsou spouštěny asynchronně.</span><span class="sxs-lookup"><span data-stu-id="8c042-154">Web API now supports an easy way to create filters that execute asynchronously.</span></span> <span data-ttu-id="8c042-155">Tato funkce je užitečná je váš filtr se musí provést asynchronní akci, jako je například přístup a databáze.</span><span class="sxs-lookup"><span data-stu-id="8c042-155">This feature is useful is your filter needs to perform an async action, such as access a database.</span></span> <span data-ttu-id="8c042-156">Dříve Pokud chcete vytvořit filtr asynchronní, jste museli implementovat rozhraní filtr sami, protože základní třídy filtr dostupná jenom v případě synchronních metod.</span><span class="sxs-lookup"><span data-stu-id="8c042-156">Previously, to create an async filter, you had to implement the filter interface yourself, because the filter base classes only exposed synchronous methods.</span></span> <span data-ttu-id="8c042-157">Nyní můžete přepsat virtuální `On*Async` filtru metody základní třídy.</span><span class="sxs-lookup"><span data-stu-id="8c042-157">Now you can override the virtual `On*Async` methods of the filter base class.</span></span>

<span data-ttu-id="8c042-158">Příklad:</span><span class="sxs-lookup"><span data-stu-id="8c042-158">For example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

<span data-ttu-id="8c042-159">**AuthorizationFilterAttribute**, **ActionFilterAttribute**, a **ExceptionFilterAttribute** všechny třídy podporují asynchronní ve webové rozhraní API 2.1.</span><span class="sxs-lookup"><span data-stu-id="8c042-159">The **AuthorizationFilterAttribute**, **ActionFilterAttribute**, and **ExceptionFilterAttribute** classes all support async in Web API 2.1.</span></span>

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a><span data-ttu-id="8c042-160">Analýza kódu pro formátování knihovna klienta dotazu</span><span class="sxs-lookup"><span data-stu-id="8c042-160">Query Parsing for the Client Formatting Library</span></span>

<span data-ttu-id="8c042-161">Dříve **System.Net.Http.Formatting** nepodporuje analýzu a aktualizace dotazů identifikátor URI pro kód na straně serveru, ale je ekvivalentní v přenosné knihovně chybí tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="8c042-161">Previously, **System.Net.Http.Formatting** supported parsing and updating URI queries for server-side code, but the equivalent portable library was missing this feature.</span></span> <span data-ttu-id="8c042-162">Ve webové rozhraní API 2.1 klientská aplikace můžete nyní snadno analyzovat a aktualizovat řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="8c042-162">In Web API 2.1, a client application can now easily parse and update a query string.</span></span>

<span data-ttu-id="8c042-163">Následující příklady ukazují, jak analyzovat, upravit a generovat dotazy identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="8c042-163">The following examples show how to parse, modify, and generate URI queries.</span></span> <span data-ttu-id="8c042-164">(Příklady ukazují konzolovou aplikaci kvůli zjednodušení.)</span><span class="sxs-lookup"><span data-stu-id="8c042-164">(The examples show a console application for simplicity.)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="8c042-165">Známé problémy a změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="8c042-165">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="8c042-166">Tato část popisuje známé problémy a rozbíjející změny ASP.NET Web API 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="8c042-166">This section describes known issues and breaking changes in the ASP.NET Web API 2.1 RTM.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="8c042-167">Směrování atributů</span><span class="sxs-lookup"><span data-stu-id="8c042-167">Attribute Routing</span></span>

<span data-ttu-id="8c042-168">Nejednoznačnosti v atributu směrování odpovídá nyní nahlásit chybu spíše než vyberete první shoda.</span><span class="sxs-lookup"><span data-stu-id="8c042-168">Ambiguities in attribute routing matches now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="8c042-169">Atribut trasy mají zakázáno používat *{controller}* parametr a používat *{action}* umístí parametr trasy na akce.</span><span class="sxs-lookup"><span data-stu-id="8c042-169">Attribute routes are prohibited from using the *{controller}* parameter, and from using the *{action}* parameter on routes placed on actions.</span></span> <span data-ttu-id="8c042-170">Tyto parametry velmi pravděpodobné, že by způsobit nejednoznačnost.</span><span class="sxs-lookup"><span data-stu-id="8c042-170">These parameters would very likely cause ambiguities.</span></span>

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="8c042-171">Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.1 balíčků následek 5.0 balíčky, které už neexistují v projektu</span><span class="sxs-lookup"><span data-stu-id="8c042-171">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="8c042-172">Aktualizují se balíčky NuGet pro ASP.NET Web API 2.1 RTM nelze aktualizovat nástroje sady Visual Studio, jako je například technologie ASP.NET generování uživatelského rozhraní nebo šablonu projektu webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8c042-172">Updating NuGet packages for ASP.NET Web API 2.1 RTM does not update the Visual Studio tools, such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="8c042-173">Používají předchozí verze balíčky runtime ASP.NET (5.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="8c042-173">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="8c042-174">Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (5.0.0.0) požadované balíčky, pokud již nejsou k dispozici ve vašich projektech.</span><span class="sxs-lookup"><span data-stu-id="8c042-174">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="8c042-175">ASP.NET generování uživatelského rozhraní v aplikaci Visual Studio 2013 RTM a Update 1, ale nepřepisuje nejnovější balíčky v projektech.</span><span class="sxs-lookup"><span data-stu-id="8c042-175">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span>

<span data-ttu-id="8c042-176">Pokud používáte ASP.NET generování uživatelského rozhraní po aktualizaci balíčků pro webové rozhraní API 2.1 nebo technologie ASP.NET MVC 5.1, ujistěte se, že verze webového rozhraní API a MVC jsou konzistentní vzhledem k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="8c042-176">If you use ASP.NET scaffolding after updating the packages to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and MVC are consistent.</span></span>

### <a name="type-renames"></a><span data-ttu-id="8c042-177">Typ přejmenování</span><span class="sxs-lookup"><span data-stu-id="8c042-177">Type Renames</span></span>

<span data-ttu-id="8c042-178">Některé typy používané pro rozšíření směrování atributů byly přejmenovány z verze RC na 2.1 RTM.</span><span class="sxs-lookup"><span data-stu-id="8c042-178">Some of the types used for attribute routing extensibility were renamed from the RC to the 2.1 RTM.</span></span>

| <span data-ttu-id="8c042-179">Starý název typu (2.1 RC)</span><span class="sxs-lookup"><span data-stu-id="8c042-179">Old type name (2.1 RC)</span></span> | <span data-ttu-id="8c042-180">Název nového typu (2.1 RTM)</span><span class="sxs-lookup"><span data-stu-id="8c042-180">New Type Name (2.1 RTM)</span></span> |
| --- | --- |
| <span data-ttu-id="8c042-181">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="8c042-181">IDirectRouteProvider</span></span> | <span data-ttu-id="8c042-182">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="8c042-182">IDirectRouteFactory</span></span> |
| <span data-ttu-id="8c042-183">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="8c042-183">RouteProviderAttribute</span></span> | <span data-ttu-id="8c042-184">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="8c042-184">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="8c042-185">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="8c042-185">DirectRouteProviderContext</span></span> | <span data-ttu-id="8c042-186">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="8c042-186">DirectRouteFactoryContext</span></span> |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a><span data-ttu-id="8c042-187">Filtry výjimek rozbalení není agregační výjimek vyvolaných v asynchronní akce</span><span class="sxs-lookup"><span data-stu-id="8c042-187">Exception filters do not unwrap aggregate exceptions thrown in async actions</span></span>

<span data-ttu-id="8c042-188">Dříve pokud asynchronní akce došlo **AggregateException**, filtru výjimky by rozbalení výjimku, a **OnException** byste získali základní výjimku.</span><span class="sxs-lookup"><span data-stu-id="8c042-188">Previously, if an async action threw an **AggregateException**, an exception filter would unwrap the exception, and **OnException** would get the base exception.</span></span> <span data-ttu-id="8c042-189">V 2.1, filtru výjimek není rozbalení, a **OnException** získá původní **AggregateException**.</span><span class="sxs-lookup"><span data-stu-id="8c042-189">In 2.1, the exception filter does not unwrap it, and **OnException** gets the original **AggregateException**.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="8c042-190">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="8c042-190">Bug Fixes</span></span>

<span data-ttu-id="8c042-191">Tato verze rovněž obsahuje několik oprav chyb.</span><span class="sxs-lookup"><span data-stu-id="8c042-191">This release also includes several bug fixes.</span></span> <span data-ttu-id="8c042-192">Úplný seznam najdete:</span><span class="sxs-lookup"><span data-stu-id="8c042-192">You can find the complete list here:</span></span>

- [<span data-ttu-id="8c042-193">5.1.0 balíček</span><span class="sxs-lookup"><span data-stu-id="8c042-193">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="8c042-194">5.1.1 balíček</span><span class="sxs-lookup"><span data-stu-id="8c042-194">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<span data-ttu-id="8c042-195">5.1.2 balíček obsahuje aktualizace IntelliSense, ale žádné opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="8c042-195">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
