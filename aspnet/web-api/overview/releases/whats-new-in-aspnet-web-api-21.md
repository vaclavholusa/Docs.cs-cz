---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: Co je nového v rozhraní ASP.NET Web API 2.1 | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 6c5a73b95955663d76238e88009ca700f9ace010
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753038"
---
<a name="whats-new-in-aspnet-web-api-21"></a>Co je nového v rozhraní ASP.NET Web API 2.1
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového v ASP.NET Web API 2.1.

- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Nové funkce v rozhraní ASP.NET Web API 2.1](#new-features)

    - [Zpracování chyb globální](#global-error)
    - [Atribut směrování vylepšení](#attribute-routing)
    - [Vylepšení stránky nápovědy](#help-page)
    - [Podpora IgnoreRoute](#ignoreroute)
    - [Formátovací modul typu média BSON](#bson)
    - [Lepší podporu pro asynchronní filtry](#async-filters)
    - [Analýza kódu pro formátování knihovna klienta dotazu](#query-parsing)
- [Známé problémy a změny způsobující chyby](#known-issues)
- [Opravy chyb](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet. Postupujte podle všech balíčků modulu runtime [Semantic Versioning](http://semver.org/) specifikace. Nejnovější balíček ASP.NET Web API 2.1 RTM má následující verzi: "5.1.2". Můžete nainstalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Tato verze rovněž obsahuje odpovídající lokalizované balíčky NuGet.

Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET Web API 2.1 RTM jsou k dispozici z webu technologie ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>Nové funkce v rozhraní ASP.NET Web API 2.1

<a id="global-error"></a>
### <a name="global-error-handling"></a>Zpracování chyb globální

Prostřednictvím jedné centrální mechanismus můžete nyní protokolovat všechny neošetřené výjimky a je možné přizpůsobit chování neošetřené výjimky.

Rozhraní framework podporuje více protokolovačů výjimek, které najdete v článku neošetřené výjimky a informace o kontextu, ve kterém k němu došlo, jako je například požadavek zpracovávaný při.

Například následující kód používá System.Diagnostics.TraceSource do protokolu všechny neošetřené výjimky:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Můžete také nahradit výchozí obslužnou rutinu výjimky, takže můžete plně přizpůsobit zprávy s odpovědí HTTP, který se odešle, když k neošetřené výjimce dojde.

Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , která protokoluje všechny neošetřené výjimky pomocí oblíbených rozhraní ELMAH.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Atribut směrování vylepšení

Směrování atributů podporuje omezení, povolení správy verzí a výběr založeným na hlavičkách trasy. Mnoho aspektů trasy atributů jsou také nyní přizpůsobit prostřednictvím **IDirectRouteFactory** rozhraní a **RouteFactoryAttribute** třídy. Předpona trasy je rozšiřitelný prostřednictvím **IRoutePrefix** rozhraní a **RoutePrefixAttribute** třídy.

Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) omezení, která používá dynamicky řadiče filtrovat podle verze api-version záhlaví HTTP.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Vylepšení stránky nápovědy

Webové rozhraní API 2.1 obsahuje následující vylepšení [stránky nápovědy k rozhraní API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Dokumentace ke službě jednotlivé vlastnosti parametry nebo návratové typy akcí.
- Dokumentace ke službě anotacemi dat modelu.

Návrh uživatelského rozhraní stránky nápovědy byla také aktualizována, aby tyto změny.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>Podpora IgnoreRoute

Webové rozhraní API 2.1 podporuje ignoruje vzory adresy URL webového rozhraní API směrování přes sadu **IgnoreRoute** rozšiřující metody na **HttpRouteCollection**. Tyto metody způsobit, že webové rozhraní API ignorovat všechny adresy URL, které odpovídají zadané šablony a povolit hostitele použít další zpracování, pokud je to vhodné.

Následující příklad ignoruje identifikátory URI, které začínají &quot;obsah&quot; segmentu:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>Formátovací modul typu média BSON

Webové rozhraní API teď podporuje [BSON](http://bsonspec.org/) přenosový formát, na straně klienta i na serveru.

Pokud chcete povolit BSON na straně serveru, přidejte **BsonMediaTypeFormatter** do kolekce formátovacích modulů:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

Zde je, jak klient .NET mohou využívat formátu BSON:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Uvádíme [ukázka](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) , který ukazuje na straně klienta i serveru.

Další informace najdete v tématu [podpora formátu BSON ve webové rozhraní API 2.1](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Lepší podporu pro asynchronní filtry

Webové rozhraní API teď podporuje snadný způsob, jak vytvářet filtry, které jsou spouštěny asynchronně. Tato funkce je užitečná je váš filtr se musí provést asynchronní akci, jako je například přístup a databáze. Dříve Pokud chcete vytvořit filtr asynchronní, jste museli implementovat rozhraní filtr sami, protože základní třídy filtr dostupná jenom v případě synchronních metod. Nyní můžete přepsat virtuální `On*Async` filtru metody základní třídy.

Příklad:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**, **ActionFilterAttribute**, a **ExceptionFilterAttribute** všechny třídy podporují asynchronní ve webové rozhraní API 2.1.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Analýza kódu pro formátování knihovna klienta dotazu

Dříve **System.Net.Http.Formatting** nepodporuje analýzu a aktualizace dotazů identifikátor URI pro kód na straně serveru, ale je ekvivalentní v přenosné knihovně chybí tuto funkci. Ve webové rozhraní API 2.1 klientská aplikace můžete nyní snadno analyzovat a aktualizovat řetězec dotazu.

Následující příklady ukazují, jak analyzovat, upravit a generovat dotazy identifikátoru URI. (Příklady ukazují konzolovou aplikaci kvůli zjednodušení.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

Tato část popisuje známé problémy a rozbíjející změny ASP.NET Web API 2.1 RTM.

### <a name="attribute-routing"></a>Směrování atributů

Nejednoznačnosti v atributu směrování odpovídá nyní nahlásit chybu spíše než vyberete první shoda.

Atribut trasy mají zakázáno používat *{controller}* parametr a používat *{action}* umístí parametr trasy na akce. Tyto parametry velmi pravděpodobné, že by způsobit nejednoznačnost.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.1 balíčků následek 5.0 balíčky, které už neexistují v projektu

Aktualizují se balíčky NuGet pro ASP.NET Web API 2.1 RTM nelze aktualizovat nástroje sady Visual Studio, jako je například technologie ASP.NET generování uživatelského rozhraní nebo šablonu projektu webové aplikace ASP.NET. Používají předchozí verze balíčky runtime ASP.NET (5.0.0.0). Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (5.0.0.0) požadované balíčky, pokud již nejsou k dispozici ve vašich projektech. ASP.NET generování uživatelského rozhraní v aplikaci Visual Studio 2013 RTM a Update 1, ale nepřepisuje nejnovější balíčky v projektech.

Pokud používáte ASP.NET generování uživatelského rozhraní po aktualizaci balíčků pro webové rozhraní API 2.1 nebo technologie ASP.NET MVC 5.1, ujistěte se, že verze webového rozhraní API a MVC jsou konzistentní vzhledem k aplikacím.

### <a name="type-renames"></a>Typ přejmenování

Některé typy používané pro rozšíření směrování atributů byly přejmenovány z verze RC na 2.1 RTM.

| Starý název typu (2.1 RC) | Název nového typu (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Filtry výjimek rozbalení není agregační výjimek vyvolaných v asynchronní akce

Dříve pokud asynchronní akce došlo **AggregateException**, filtru výjimky by rozbalení výjimku, a **OnException** byste získali základní výjimku. V 2.1, filtru výjimek není rozbalení, a **OnException** získá původní **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Opravy chyb

Tato verze rovněž obsahuje několik oprav chyb. Úplný seznam najdete:

- [5.1.0 balíček](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 balíček](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 balíček obsahuje aktualizace IntelliSense, ale žádné opravy chyb.
